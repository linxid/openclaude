---
title: "Amazon Bedrock 上的 Claude Code"
description: "了解如何通过 Amazon Bedrock 配置 Claude Code，包括设置、IAM 配置和故障排除。"
---

## 前置条件

在使用 Bedrock 配置 Claude Code 之前，请确保您拥有：

* 启用了 Bedrock 访问权限的 AWS 账户
* 在 Bedrock 中访问所需的 Claude 模型（例如 Claude Sonnet 4.5）
* 已安装并配置 AWS CLI（可选 - 仅在您没有其他获取凭证的机制时需要）
* 适当的 IAM 权限

## 设置

### 1. 提交用例详情

Anthropic 模型的首次用户需要在调用模型之前提交用例详情。这在每个账户中只需进行一次。

1. 确保您拥有正确的 IAM 权限（请参阅下面的更多信息）
2. 导航到 [Amazon Bedrock 控制台](https://console.aws.amazon.com/bedrock/)
3. 选择 **Chat/Text playground**
4. 选择任何 Anthropic 模型，您将被提示填写用例表单

### 2. 配置 AWS 凭证

Claude Code 使用默认的 AWS SDK 凭证链。使用以下方法之一设置您的凭证：

**选项 A：AWS CLI 配置**

```bash  theme={null}
aws configure
```

**选项 B：环境变量（访问密钥）**

```bash  theme={null}
export AWS_ACCESS_KEY_ID=your-access-key-id
export AWS_SECRET_ACCESS_KEY=your-secret-access-key
export AWS_SESSION_TOKEN=your-session-token
```

**选项 C：环境变量（SSO 配置文件）**

```bash  theme={null}
aws sso login --profile=<your-profile-name>

export AWS_PROFILE=your-profile-name
```

**选项 D：AWS 管理控制台凭证**

```bash  theme={null}
aws login
```

[了解更多](https://docs.aws.amazon.com/signin/latest/userguide/command-line-sign-in.html)关于 `aws login`。

**选项 E：Bedrock API 密钥**

```bash  theme={null}
export AWS_BEARER_TOKEN_BEDROCK=your-bedrock-api-key
```

Bedrock API 密钥提供了一种更简单的身份验证方法，无需完整的 AWS 凭证。[了解更多关于 Bedrock API 密钥的信息](https://aws.amazon.com/blogs/machine-learning/accelerate-ai-development-with-amazon-bedrock-api-keys/)。

#### 高级凭证配置

Claude Code 支持 AWS SSO 和企业身份提供商的自动凭证刷新。将这些设置添加到您的 Claude Code 设置文件中（请参阅 [Settings](/zh-CN/settings) 了解文件位置）。

当 Claude Code 检测到您的 AWS 凭证已过期（基于本地时间戳或当 Bedrock 返回凭证错误时），它将自动运行您配置的 `awsAuthRefresh` 和/或 `awsCredentialExport` 命令来获取新凭证，然后重试请求。

##### 示例配置

```json  theme={null}
{
  "awsAuthRefresh": "aws sso login --profile myprofile",
  "env": {
    "AWS_PROFILE": "myprofile"
  }
}
```

##### 配置设置说明

**`awsAuthRefresh`**：用于修改 `.aws` 目录的命令，例如更新凭证、SSO 缓存或配置文件。命令的输出会显示给用户，但不支持交互式输入。这适用于基于浏览器的 SSO 流程，其中 CLI 显示 URL 或代码，您在浏览器中完成身份验证。

**`awsCredentialExport`**：仅在无法修改 `.aws` 且必须直接返回凭证时使用。输出会被静默捕获，不会显示给用户。命令必须输出以下格式的 JSON：

```json  theme={null}
{
  "Credentials": {
    "AccessKeyId": "value",
    "SecretAccessKey": "value",
    "SessionToken": "value"
  }
}
```

### 3. 配置 Claude Code

设置以下环境变量以启用 Bedrock：

```bash  theme={null}
# 启用 Bedrock 集成
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1  # 或您首选的区域

# 可选：覆盖小型/快速模型 (Haiku) 的区域
export ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION=us-west-2
```

为 Claude Code 启用 Bedrock 时，请记住以下几点：

* `AWS_REGION` 是必需的环境变量。Claude Code 不会从 `.aws` 配置文件中读取此设置。
* 使用 Bedrock 时，`/login` 和 `/logout` 命令被禁用，因为身份验证通过 AWS 凭证处理。
* 您可以使用设置文件来处理环境变量，如 `AWS_PROFILE`，这样您不想将其泄露给其他进程。请参阅 [Settings](/zh-CN/settings) 了解更多信息。

### 4. 模型配置

Claude Code 为 Bedrock 使用这些默认模型：

| 模型类型    | 默认值                                                |
| :------ | :------------------------------------------------- |
| 主模型     | `global.anthropic.claude-sonnet-4-5-20250929-v1:0` |
| 小型/快速模型 | `us.anthropic.claude-haiku-4-5-20251001-v1:0`      |

<Note>
  对于 Bedrock 用户，Claude Code 不会自动从 Haiku 3.5 升级到 Haiku 4.5。要手动切换到较新的 Haiku 模型，请将 `ANTHROPIC_DEFAULT_HAIKU_MODEL` 环境变量设置为完整的模型名称（例如，`us.anthropic.claude-haiku-4-5-20251001-v1:0`）。
</Note>

要自定义模型，请使用以下方法之一：

```bash  theme={null}
# 使用推理配置文件 ID
export ANTHROPIC_MODEL='global.anthropic.claude-sonnet-4-5-20250929-v1:0'
export ANTHROPIC_SMALL_FAST_MODEL='us.anthropic.claude-haiku-4-5-20251001-v1:0'

# 使用应用推理配置文件 ARN
export ANTHROPIC_MODEL='arn:aws:bedrock:us-east-2:your-account-id:application-inference-profile/your-model-id'

# 可选：如果需要，禁用 prompt caching
export DISABLE_PROMPT_CACHING=1
```

<Note>[Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 可能不在所有区域可用。</Note>

### 5. 输出令牌配置

这些是 Claude Code 与 Amazon Bedrock 的推荐令牌设置：

```bash  theme={null}
# Bedrock 的推荐输出令牌设置
export CLAUDE_CODE_MAX_OUTPUT_TOKENS=4096
export MAX_THINKING_TOKENS=1024
```

**为什么选择这些值：**

* **`CLAUDE_CODE_MAX_OUTPUT_TOKENS=4096`**：Bedrock 的降速限流逻辑将 4096 令牌设置为 `max_token` 惩罚的最小值。设置更低的值不会降低成本，但可能会截断长工具使用，导致 Claude Code 代理循环持续失败。Claude Code 在没有扩展思考的情况下通常使用少于 4096 个输出令牌，但对于涉及大量文件创建或 Write 工具使用的任务可能需要这个余量。

* **`MAX_THINKING_TOKENS=1024`**：这为扩展思考提供了空间，而不会截断工具使用响应，同时仍然保持专注的推理链。这种平衡有助于防止对编码任务不总是有帮助的轨迹变化。

## IAM 配置

创建具有 Claude Code 所需权限的 IAM 策略：

```json  theme={null}
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowModelAndInferenceProfileAccess",
      "Effect": "Allow",
      "Action": [
        "bedrock:InvokeModel",
        "bedrock:InvokeModelWithResponseStream",
        "bedrock:ListInferenceProfiles"
      ],
      "Resource": [
        "arn:aws:bedrock:*:*:inference-profile/*",
        "arn:aws:bedrock:*:*:application-inference-profile/*",
        "arn:aws:bedrock:*:*:foundation-model/*"
      ]
    },
    {
      "Sid": "AllowMarketplaceSubscription",
      "Effect": "Allow",
      "Action": [
        "aws-marketplace:ViewSubscriptions",
        "aws-marketplace:Subscribe"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:CalledViaLast": "bedrock.amazonaws.com"
        }
      }
    }
  ]
}
```

为了获得更严格的权限，您可以将 Resource 限制为特定的推理配置文件 ARN。

有关详细信息，请参阅 [Bedrock IAM 文档](https://docs.aws.amazon.com/bedrock/latest/userguide/security-iam.html)。

<Note>
  我们建议为 Claude Code 创建一个专用的 AWS 账户，以简化成本跟踪和访问控制。
</Note>

## AWS Guardrails

[Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html) 让您为 Claude Code 实现内容过滤。在 [Amazon Bedrock 控制台](https://console.aws.amazon.com/bedrock/) 中创建 Guardrail，发布一个版本，然后将 Guardrail 标头添加到您的 [settings 文件](/zh-CN/settings)。如果您使用跨区域推理配置文件，请在您的 Guardrail 上启用跨区域推理。

示例配置：

```json  theme={null}
{
  "env": {
    "ANTHROPIC_CUSTOM_HEADERS": "X-Amzn-Bedrock-GuardrailIdentifier: your-guardrail-id\nX-Amzn-Bedrock-GuardrailVersion: 1"
  }
}
```

## 故障排除

如果您遇到区域问题：

* 检查模型可用性：`aws bedrock list-inference-profiles --region your-region`
* 切换到支持的区域：`export AWS_REGION=us-east-1`
* 考虑使用推理配置文件进行跨区域访问

如果您收到错误"on-demand throughput isn't supported"：

* 将模型指定为 [inference profile](https://docs.aws.amazon.com/bedrock/latest/userguide/inference-profiles-support.html) ID

Claude Code 使用 Bedrock [Invoke API](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_runtime_InvokeModelWithResponseStream.html)，不支持 Converse API。

## 其他资源

* [Bedrock 文档](https://docs.aws.amazon.com/bedrock/)
* [Bedrock 定价](https://aws.amazon.com/bedrock/pricing/)
* [Bedrock 推理配置文件](https://docs.aws.amazon.com/bedrock/latest/userguide/inference-profiles-support.html)
* [Amazon Bedrock 上的 Claude Code：快速设置指南](https://community.aws/content/2tXkZKrZzlrlu0KfH8gST5Dkppq/claude-code-on-amazon-bedrock-quick-setup-guide)
* [Claude Code 监控实现 (Bedrock)](https://github.com/aws-solutions-library-samples/guidance-for-claude-code-with-amazon-bedrock/blob/main/assets/docs/MONITORING.md)
