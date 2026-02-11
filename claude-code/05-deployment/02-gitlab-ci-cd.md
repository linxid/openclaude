---
title: "Claude Code GitLab CI/CD"
description: "了解如何将 Claude Code 集成到您的 GitLab CI/CD 开发工作流中"
---

<Info>
  Claude Code for GitLab CI/CD 目前处于测试阶段。随着我们完善体验，功能和特性可能会发生变化。

  此集成由 GitLab 维护。如需支持，请参阅以下 [GitLab issue](https://gitlab.com/gitlab-org/gitlab/-/issues/573776)。
</Info>

<Note>
  此集成构建在 [Claude Code CLI 和 Agent SDK](https://platform.claude.com/docs/zh-CN/agent-sdk/overview) 之上，支持在您的 CI/CD 作业和自定义自动化工作流中以编程方式使用 Claude。
</Note>

## 为什么在 GitLab 中使用 Claude Code？

* **即时 MR 创建**：描述您的需求，Claude 会提议一个完整的 MR，包含更改和解释
* **自动化实现**：使用单个命令或提及将问题转化为可工作的代码
* **项目感知**：Claude 遵循您的 `CLAUDE.md` 指南和现有代码模式
* **简单设置**：向 `.gitlab-ci.yml` 添加一个作业和一个掩码 CI/CD 变量
* **企业就绪**：选择 Claude API、AWS Bedrock 或 Google Vertex AI 以满足数据驻留和采购需求
* **默认安全**：在您的 GitLab runners 中运行，具有您的分支保护和批准

## 工作原理

Claude Code 使用 GitLab CI/CD 在隔离的作业中运行 AI 任务，并通过 MR 将结果提交回来：

1. **事件驱动的编排**：GitLab 监听您选择的触发器（例如，在问题、MR 或审查线程中提及 `@claude` 的评论）。该作业从线程和存储库收集上下文，从该输入构建提示，并运行 Claude Code。

2. **提供商抽象**：使用适合您环境的提供商：
   * Claude API (SaaS)
   * AWS Bedrock（基于 IAM 的访问、跨区域选项）
   * Google Vertex AI（GCP 原生、Workload Identity Federation）

3. **沙箱执行**：每次交互都在具有严格网络和文件系统规则的容器中运行。Claude Code 强制执行工作区范围的权限以限制写入。每项更改都通过 MR 流动，以便审查者可以看到差异并且批准仍然适用。

选择区域端点以减少延迟并满足数据主权要求，同时使用现有的云协议。

## Claude 可以做什么？

Claude Code 支持强大的 CI/CD 工作流，改变您处理代码的方式：

* 从问题描述或评论创建和更新 MR
* 分析性能回归并提议优化
* 直接在分支中实现功能，然后打开 MR
* 修复由测试或评论识别的错误和回归
* 响应后续评论以迭代所请求的更改

## 设置

### 快速设置

最快的入门方式是向您的 `.gitlab-ci.yml` 添加一个最小作业，并将您的 API 密钥设置为掩码变量。

1. **添加掩码 CI/CD 变量**
   * 转到 **Settings** → **CI/CD** → **Variables**
   * 添加 `ANTHROPIC_API_KEY`（掩码，根据需要保护）

2. **向 `.gitlab-ci.yml` 添加 Claude 作业**

```yaml  theme={null}
stages:
  - ai

claude:
  stage: ai
  image: node:24-alpine3.21
  # 调整规则以适应您想要触发作业的方式：
  # - 手动运行
  # - 合并请求事件
  # - 当评论包含 '@claude' 时的 web/API 触发
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  variables:
    GIT_STRATEGY: fetch
  before_script:
    - apk update
    - apk add --no-cache git curl bash
    - curl -fsSL https://claude.ai/install.sh | bash
  script:
    # 可选：如果您的设置提供了 GitLab MCP 服务器，请启动它
    - /bin/gitlab-mcp-server || true
    # 通过 web/API 触发器使用 AI_FLOW_* 变量调用时使用上下文有效负载
    - echo "$AI_FLOW_INPUT for $AI_FLOW_CONTEXT on $AI_FLOW_EVENT"
    - >
      claude
      -p "${AI_FLOW_INPUT:-'Review this MR and implement the requested changes'}"
      --permission-mode acceptEdits
      --allowedTools "Bash Read Edit Write mcp__gitlab"
      --debug
```

添加作业和 `ANTHROPIC_API_KEY` 变量后，通过从 **CI/CD** → **Pipelines** 手动运行作业进行测试，或从 MR 触发它，让 Claude 在分支中提议更新并在需要时打开 MR。

<Note>
  要改为在 AWS Bedrock 或 Google Vertex AI 上运行而不是 Claude API，请参阅下面的 [使用 AWS Bedrock 和 Google Vertex AI](#using-with-aws-bedrock--google-vertex-ai) 部分了解身份验证和环境设置。
</Note>

### 手动设置（建议用于生产）

如果您更喜欢更受控的设置或需要企业提供商：

1. **配置提供商访问**：
   * **Claude API**：创建并将 `ANTHROPIC_API_KEY` 存储为掩码 CI/CD 变量
   * **AWS Bedrock**：**配置 GitLab** → **AWS OIDC** 并为 Bedrock 创建 IAM 角色
   * **Google Vertex AI**：**为 GitLab 配置 Workload Identity Federation** → **GCP**

2. **为 GitLab API 操作添加项目凭证**：
   * 默认使用 `CI_JOB_TOKEN`，或创建具有 `api` 范围的项目访问令牌
   * 如果使用 PAT，则存储为 `GITLAB_ACCESS_TOKEN`（掩码）

3. **向 `.gitlab-ci.yml` 添加 Claude 作业**（参见下面的示例）

4. **（可选）启用提及驱动的触发器**：
   * 为"Comments (notes)"添加项目 webhook 到您的事件监听器（如果您使用的话）
   * 当评论包含 `@claude` 时，让监听器使用 `AI_FLOW_INPUT` 和 `AI_FLOW_CONTEXT` 等变量调用管道触发 API

## 示例用例

### 将问题转化为 MR

在问题评论中：

```
@claude implement this feature based on the issue description
```

Claude 分析问题和代码库，在分支中编写更改，并打开 MR 供审查。

### 获取实现帮助

在 MR 讨论中：

```
@claude suggest a concrete approach to cache the results of this API call
```

Claude 提议更改，添加具有适当缓存的代码，并更新 MR。

### 快速修复错误

在问题或 MR 评论中：

```
@claude fix the TypeError in the user dashboard component
```

Claude 定位错误，实现修复，并更新分支或打开新的 MR。

## 使用 AWS Bedrock 和 Google Vertex AI

对于企业环境，您可以在云基础设施上完全运行 Claude Code，具有相同的开发者体验。

<Tabs>
  <Tab title="AWS Bedrock">
    ### 前置条件

    在使用 AWS Bedrock 设置 Claude Code 之前，您需要：

    1. 具有对所需 Claude 模型的 Amazon Bedrock 访问权限的 AWS 账户
    2. 在 AWS IAM 中配置为 OIDC 身份提供商的 GitLab
    3. 具有 Bedrock 权限和信任策略的 IAM 角色，限制为您的 GitLab 项目/refs
    4. 用于角色假设的 GitLab CI/CD 变量：
       * `AWS_ROLE_TO_ASSUME`（角色 ARN）
       * `AWS_REGION`（Bedrock 区域）

    ### 设置说明

    配置 AWS 以允许 GitLab CI 作业通过 OIDC 假设 IAM 角色（无静态密钥）。

    **必需的设置：**

    1. 启用 Amazon Bedrock 并请求访问您的目标 Claude 模型
    2. 如果尚未存在，为 GitLab 创建 IAM OIDC 提供商
    3. 创建由 GitLab OIDC 提供商信任的 IAM 角色，限制为您的项目和受保护的 refs
    4. 为 Bedrock invoke API 附加最小权限

    **需要存储在 CI/CD 变量中的必需值：**

    * `AWS_ROLE_TO_ASSUME`
    * `AWS_REGION`

    在 Settings → CI/CD → Variables 中添加变量：

    ```yaml  theme={null}
    # 对于 AWS Bedrock：
    - AWS_ROLE_TO_ASSUME
    - AWS_REGION
    ```

    使用上面的 AWS Bedrock 作业示例在运行时交换 GitLab 作业令牌以获取临时 AWS 凭证。
  </Tab>

  <Tab title="Google Vertex AI">
    ### 前置条件

    在使用 Google Vertex AI 设置 Claude Code 之前，您需要：

    1. 具有以下条件的 Google Cloud 项目：
       * 启用了 Vertex AI API
       * 配置了 Workload Identity Federation 以信任 GitLab OIDC
    2. 仅具有所需 Vertex AI 角色的专用服务账户
    3. 用于 WIF 的 GitLab CI/CD 变量：
       * `GCP_WORKLOAD_IDENTITY_PROVIDER`（完整资源名称）
       * `GCP_SERVICE_ACCOUNT`（服务账户电子邮件）

    ### 设置说明

    配置 Google Cloud 以允许 GitLab CI 作业通过 Workload Identity Federation 模拟服务账户。

    **必需的设置：**

    1. 启用 IAM Credentials API、STS API 和 Vertex AI API
    2. 为 GitLab OIDC 创建 Workload Identity Pool 和提供商
    3. 创建具有 Vertex AI 角色的专用服务账户
    4. 授予 WIF 主体权限以模拟服务账户

    **需要存储在 CI/CD 变量中的必需值：**

    * `GCP_WORKLOAD_IDENTITY_PROVIDER`
    * `GCP_SERVICE_ACCOUNT`

    在 Settings → CI/CD → Variables 中添加变量：

    ```yaml  theme={null}
    # 对于 Google Vertex AI：
    - GCP_WORKLOAD_IDENTITY_PROVIDER
    - GCP_SERVICE_ACCOUNT
    - CLOUD_ML_REGION（例如，us-east5）
    ```

    使用上面的 Google Vertex AI 作业示例在不存储密钥的情况下进行身份验证。
  </Tab>
</Tabs>

## 配置示例

以下是您可以适配到管道中的现成代码片段。

### 基本 .gitlab-ci.yml（Claude API）

```yaml  theme={null}
stages:
  - ai

claude:
  stage: ai
  image: node:24-alpine3.21
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  variables:
    GIT_STRATEGY: fetch
  before_script:
    - apk update
    - apk add --no-cache git curl bash
    - curl -fsSL https://claude.ai/install.sh | bash
  script:
    - /bin/gitlab-mcp-server || true
    - >
      claude
      -p "${AI_FLOW_INPUT:-'Summarize recent changes and suggest improvements'}"
      --permission-mode acceptEdits
      --allowedTools "Bash Read Edit Write mcp__gitlab"
      --debug
  # Claude Code 将使用 CI/CD 变量中的 ANTHROPIC_API_KEY
```

### AWS Bedrock 作业示例（OIDC）

**前置条件：**

* 启用了 Amazon Bedrock 并可访问您选择的 Claude 模型
* 在 AWS 中配置了 GitLab OIDC，具有信任您的 GitLab 项目和 refs 的角色
* 具有 Bedrock 权限的 IAM 角色（建议最小权限）

**必需的 CI/CD 变量：**

* `AWS_ROLE_TO_ASSUME`：Bedrock 访问的 IAM 角色的 ARN
* `AWS_REGION`：Bedrock 区域（例如，`us-west-2`）

```yaml  theme={null}
claude-bedrock:
  stage: ai
  image: node:24-alpine3.21
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'
  before_script:
    - apk add --no-cache bash curl jq git python3 py3-pip
    - pip install --no-cache-dir awscli
    - curl -fsSL https://claude.ai/install.sh | bash
    # 交换 GitLab OIDC 令牌以获取 AWS 凭证
    - export AWS_WEB_IDENTITY_TOKEN_FILE="${CI_JOB_JWT_FILE:-/tmp/oidc_token}"
    - if [ -n "${CI_JOB_JWT_V2}" ]; then printf "%s" "$CI_JOB_JWT_V2" > "$AWS_WEB_IDENTITY_TOKEN_FILE"; fi
    - >
      aws sts assume-role-with-web-identity
      --role-arn "$AWS_ROLE_TO_ASSUME"
      --role-session-name "gitlab-claude-$(date +%s)"
      --web-identity-token "file://$AWS_WEB_IDENTITY_TOKEN_FILE"
      --duration-seconds 3600 > /tmp/aws_creds.json
    - export AWS_ACCESS_KEY_ID="$(jq -r .Credentials.AccessKeyId /tmp/aws_creds.json)"
    - export AWS_SECRET_ACCESS_KEY="$(jq -r .Credentials.SecretAccessKey /tmp/aws_creds.json)"
    - export AWS_SESSION_TOKEN="$(jq -r .Credentials.SessionToken /tmp/aws_creds.json)"
  script:
    - /bin/gitlab-mcp-server || true
    - >
      claude
      -p "${AI_FLOW_INPUT:-'Implement the requested changes and open an MR'}"
      --permission-mode acceptEdits
      --allowedTools "Bash Read Edit Write mcp__gitlab"
      --debug
  variables:
    AWS_REGION: "us-west-2"
```

<Note>
  Bedrock 的模型 ID 包括特定于区域的前缀和版本后缀（例如，`us.anthropic.claude-sonnet-4-5-20250929-v1:0`）。如果您的工作流支持，通过作业配置或提示传递所需的模型。
</Note>

### Google Vertex AI 作业示例（Workload Identity Federation）

**前置条件：**

* 在您的 GCP 项目中启用了 Vertex AI API
* 配置了 Workload Identity Federation 以信任 GitLab OIDC
* 具有 Vertex AI 权限的服务账户

**必需的 CI/CD 变量：**

* `GCP_WORKLOAD_IDENTITY_PROVIDER`：完整的提供商资源名称
* `GCP_SERVICE_ACCOUNT`：服务账户电子邮件
* `CLOUD_ML_REGION`：Vertex 区域（例如，`us-east5`）

```yaml  theme={null}
claude-vertex:
  stage: ai
  image: gcr.io/google.com/cloudsdktool/google-cloud-cli:slim
  rules:
    - if: '$CI_PIPELINE_SOURCE == "web"'
  before_script:
    - apt-get update && apt-get install -y git && apt-get clean
    - curl -fsSL https://claude.ai/install.sh | bash
    # 通过 WIF 向 Google Cloud 进行身份验证（无下载的密钥）
    - >
      gcloud auth login --cred-file=<(cat <<EOF
      {
        "type": "external_account",
        "audience": "${GCP_WORKLOAD_IDENTITY_PROVIDER}",
        "subject_token_type": "urn:ietf:params:oauth:token-type:jwt",
        "service_account_impersonation_url": "https://iamcredentials.googleapis.com/v1/projects/-/serviceAccounts/${GCP_SERVICE_ACCOUNT}:generateAccessToken",
        "token_url": "https://sts.googleapis.com/v1/token"
      }
      EOF
      )
    - gcloud config set project "$(gcloud projects list --format='value(projectId)' --filter="name:${CI_PROJECT_NAMESPACE}" | head -n1)" || true
  script:
    - /bin/gitlab-mcp-server || true
    - >
      CLOUD_ML_REGION="${CLOUD_ML_REGION:-us-east5}"
      claude
      -p "${AI_FLOW_INPUT:-'Review and update code as requested'}"
      --permission-mode acceptEdits
      --allowedTools "Bash Read Edit Write mcp__gitlab"
      --debug
  variables:
    CLOUD_ML_REGION: "us-east5"
```

<Note>
  使用 Workload Identity Federation，您无需存储服务账户密钥。使用特定于存储库的信任条件和最小权限服务账户。
</Note>

## 最佳实践

### CLAUDE.md 配置

在存储库根目录创建 `CLAUDE.md` 文件以定义编码标准、审查标准和项目特定规则。Claude 在运行期间读取此文件并在提议更改时遵循您的约定。

### 安全考虑

**永远不要将 API 密钥或云凭证提交到您的存储库**。始终使用 GitLab CI/CD 变量：

* 将 `ANTHROPIC_API_KEY` 添加为掩码变量（如果需要，保护它）
* 尽可能使用提供商特定的 OIDC（无长期密钥）
* 限制作业权限和网络出口
* 像审查任何其他贡献者一样审查 Claude 的 MR

### 优化性能

* 保持 `CLAUDE.md` 专注和简洁
* 提供清晰的问题/MR 描述以减少迭代
* 配置合理的作业超时以避免失控运行
* 在可能的情况下在 runners 中缓存 npm 和包安装

### CI 成本

在 GitLab CI/CD 中使用 Claude Code 时，请注意相关成本：

* **GitLab Runner 时间**：
  * Claude 在您的 GitLab runners 上运行并消耗计算分钟数
  * 有关详细信息，请参阅您的 GitLab 计划的 runner 计费

* **API 成本**：
  * 每次 Claude 交互根据提示和响应大小消耗令牌
  * 令牌使用因任务复杂性和代码库大小而异
  * 有关详细信息，请参阅 [Anthropic 定价](https://platform.claude.com/docs/zh-CN/about-claude/pricing)

* **成本优化提示**：
  * 使用特定的 `@claude` 命令以减少不必要的轮次
  * 设置适当的 `max_turns` 和作业超时值
  * 限制并发以控制并行运行

## 安全和治理

* 每个作业都在具有受限网络访问的隔离容器中运行
* Claude 的更改通过 MR 流动，以便审查者可以看到每个差异
* 分支保护和批准规则适用于 AI 生成的代码
* Claude Code 使用工作区范围的权限来限制写入
* 成本保持在您的控制下，因为您带来自己的提供商凭证

## 故障排除

### Claude 不响应 @claude 命令

* 验证您的管道是否被触发（手动、MR 事件或通过注释事件监听器/webhook）
* 确保 CI/CD 变量（`ANTHROPIC_API_KEY` 或云提供商设置）存在且未掩码
* 检查评论是否包含 `@claude`（不是 `/claude`）以及您的提及触发器是否已配置

### 作业无法写入评论或打开 MR

* 确保 `CI_JOB_TOKEN` 对项目具有足够的权限，或使用具有 `api` 范围的项目访问令牌
* 检查 `mcp__gitlab` 工具是否在 `--allowedTools` 中启用
* 确认作业在 MR 的上下文中运行或通过 `AI_FLOW_*` 变量有足够的上下文

### 身份验证错误

* **对于 Claude API**：确认 `ANTHROPIC_API_KEY` 有效且未过期
* **对于 Bedrock/Vertex**：验证 OIDC/WIF 配置、角色模拟和密钥名称；确认区域和模型可用性

## 高级配置

### 常见参数和变量

Claude Code 支持这些常用输入：

* `prompt` / `prompt_file`：内联提供说明（`-p`）或通过文件
* `max_turns`：限制来回迭代的次数
* `timeout_minutes`：限制总执行时间
* `ANTHROPIC_API_KEY`：Claude API 所需（不用于 Bedrock/Vertex）
* 提供商特定的环境：`AWS_REGION`、Vertex 的项目/区域变量

<Note>
  确切的标志和参数可能因 `@anthropic-ai/claude-code` 的版本而异。在您的作业中运行 `claude --help` 以查看支持的选项。
</Note>

### 自定义 Claude 的行为

您可以通过两种主要方式指导 Claude：

1. **CLAUDE.md**：定义编码标准、安全要求和项目约定。Claude 在运行期间读取此文件并遵循您的规则。
2. **自定义提示**：通过作业中的 `prompt`/`prompt_file` 传递特定于任务的说明。为不同的作业使用不同的提示（例如，审查、实现、重构）。
