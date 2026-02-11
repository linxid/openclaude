---
title: "企业部署概述"
description: "了解 Claude Code 如何与各种第三方服务和基础设施集成，以满足企业部署需求。"
---

# 企业部署概述

> 了解 Claude Code 如何与各种第三方服务和基础设施集成，以满足企业部署需求。

本页面提供了可用部署选项的概述，并帮助您为组织选择正确的配置。

## 提供商对比

<table>
  <thead>
    <tr>
      <th>功能</th>
      <th>Anthropic</th>
      <th>Amazon Bedrock</th>
      <th>Google Vertex AI</th>
      <th>Microsoft Foundry</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>区域</td>
      <td>支持的[国家](https://www.anthropic.com/supported-countries)</td>
      <td>多个 AWS [区域](https://docs.aws.amazon.com/bedrock/latest/userguide/models-regions.html)</td>
      <td>多个 GCP [区域](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/locations)</td>
      <td>多个 Azure [区域](https://azure.microsoft.com/en-us/explore/global-infrastructure/products-by-region/)</td>
    </tr>

    <tr>
      <td>提示缓存</td>
      <td>默认启用</td>
      <td>默认启用</td>
      <td>默认启用</td>
      <td>默认启用</td>
    </tr>

    <tr>
      <td>身份验证</td>
      <td>API 密钥</td>
      <td>API 密钥或 AWS 凭证</td>
      <td>GCP 凭证</td>
      <td>API 密钥或 Microsoft Entra ID</td>
    </tr>

    <tr>
      <td>成本跟踪</td>
      <td>仪表板</td>
      <td>AWS Cost Explorer</td>
      <td>GCP Billing</td>
      <td>Azure Cost Management</td>
    </tr>

    <tr>
      <td>企业功能</td>
      <td>团队、使用情况监控</td>
      <td>IAM 策略、CloudTrail</td>
      <td>IAM 角色、Cloud Audit Logs</td>
      <td>RBAC 策略、Azure Monitor</td>
    </tr>
  </tbody>
</table>

## 云提供商

<CardGroup cols={3}>
  <Card title="Amazon Bedrock" icon="aws" href="/zh-CN/amazon-bedrock">
    通过 AWS 基础设施使用 Claude 模型，支持 API 密钥或基于 IAM 的身份验证以及 AWS 原生监控
  </Card>

  <Card title="Google Vertex AI" icon="google" href="/zh-CN/google-vertex-ai">
    通过 Google Cloud Platform 访问 Claude 模型，具有企业级安全性和合规性
  </Card>

  <Card title="Microsoft Foundry" icon="microsoft" href="/zh-CN/microsoft-foundry">
    通过 Azure 访问 Claude，支持 API 密钥或 Microsoft Entra ID 身份验证以及 Azure 计费
  </Card>
</CardGroup>

## 企业基础设施

<CardGroup cols={2}>
  <Card title="企业网络" icon="shield" href="/zh-CN/network-config">
    配置 Claude Code 以与您组织的代理服务器和 SSL/TLS 需求配合使用
  </Card>

  <Card title="LLM 网关" icon="server" href="/zh-CN/llm-gateway">
    部署集中式模型访问，具有使用情况跟踪、预算编制和审计日志
  </Card>
</CardGroup>

## 配置概述

Claude Code 支持灵活的配置选项，允许您组合不同的提供商和基础设施：

<Note>
  了解以下内容的区别：

  * **企业代理**：用于路由流量的 HTTP/HTTPS 代理（通过 `HTTPS_PROXY` 或 `HTTP_PROXY` 设置）
  * **LLM 网关**：处理身份验证并提供提供商兼容端点的服务（通过 `ANTHROPIC_BASE_URL`、`ANTHROPIC_BEDROCK_BASE_URL` 或 `ANTHROPIC_VERTEX_BASE_URL` 设置）

  两种配置可以同时使用。
</Note>

### 将 Bedrock 与企业代理结合使用

通过企业 HTTP/HTTPS 代理路由 Bedrock 流量：

```bash  theme={null}
# 启用 Bedrock
export CLAUDE_CODE_USE_BEDROCK=1
export AWS_REGION=us-east-1

# 配置企业代理
export HTTPS_PROXY='https://proxy.example.com:8080'
```

### 将 Bedrock 与 LLM 网关结合使用

使用提供 Bedrock 兼容端点的网关服务：

```bash  theme={null}
# 启用 Bedrock
export CLAUDE_CODE_USE_BEDROCK=1

# 配置 LLM 网关
export ANTHROPIC_BEDROCK_BASE_URL='https://your-llm-gateway.com/bedrock'
export CLAUDE_CODE_SKIP_BEDROCK_AUTH=1  # 如果网关处理 AWS 身份验证
```

### 将 Foundry 与企业代理结合使用

通过企业 HTTP/HTTPS 代理路由 Azure 流量：

```bash  theme={null}
# 启用 Microsoft Foundry
export CLAUDE_CODE_USE_FOUNDRY=1
export ANTHROPIC_FOUNDRY_RESOURCE=your-resource
export ANTHROPIC_FOUNDRY_API_KEY=your-api-key  # 或省略以使用 Entra ID 身份验证

# 配置企业代理
export HTTPS_PROXY='https://proxy.example.com:8080'
```

### 将 Foundry 与 LLM 网关结合使用

使用提供 Azure 兼容端点的网关服务：

```bash  theme={null}
# 启用 Microsoft Foundry
export CLAUDE_CODE_USE_FOUNDRY=1

# 配置 LLM 网关
export ANTHROPIC_FOUNDRY_BASE_URL='https://your-llm-gateway.com'
export CLAUDE_CODE_SKIP_FOUNDRY_AUTH=1  # 如果网关处理 Azure 身份验证
```

### 将 Vertex AI 与企业代理结合使用

通过企业 HTTP/HTTPS 代理路由 Vertex AI 流量：

```bash  theme={null}
# 启用 Vertex
export CLAUDE_CODE_USE_VERTEX=1
export CLOUD_ML_REGION=us-east5
export ANTHROPIC_VERTEX_PROJECT_ID=your-project-id

# 配置企业代理
export HTTPS_PROXY='https://proxy.example.com:8080'
```

### 将 Vertex AI 与 LLM 网关结合使用

将 Google Vertex AI 模型与 LLM 网关结合，实现集中式管理：

```bash  theme={null}
# 启用 Vertex
export CLAUDE_CODE_USE_VERTEX=1

# 配置 LLM 网关
export ANTHROPIC_VERTEX_BASE_URL='https://your-llm-gateway.com/vertex'
export CLAUDE_CODE_SKIP_VERTEX_AUTH=1  # 如果网关处理 GCP 身份验证
```

### 身份验证配置

Claude Code 在需要时使用 `ANTHROPIC_AUTH_TOKEN` 作为 `Authorization` 标头。`SKIP_AUTH` 标志（`CLAUDE_CODE_SKIP_BEDROCK_AUTH`、`CLAUDE_CODE_SKIP_VERTEX_AUTH`）用于 LLM 网关场景，其中网关处理提供商身份验证。

## 选择正确的部署配置

选择部署方法时，请考虑以下因素：

### 直接提供商访问

最适合以下组织：

* 希望最简单的设置
* 已有 AWS 或 GCP 基础设施
* 需要提供商原生监控和合规性

### 企业代理

最适合以下组织：

* 有现有的企业代理需求
* 需要流量监控和合规性
* 必须通过特定网络路径路由所有流量

### LLM 网关

最适合以下组织：

* 需要跨团队的使用情况跟踪
* 希望在模型之间动态切换
* 需要自定义速率限制或预算
* 需要集中式身份验证管理

## 调试

调试部署时：

* 使用 `claude /status` [斜杠命令](/zh-CN/slash-commands)。此命令提供对任何应用的身份验证、代理和 URL 设置的可观测性。
* 设置环境变量 `export ANTHROPIC_LOG=debug` 以记录请求。

## 组织的最佳实践

### 1. 投资文档和记忆

我们强烈建议投资文档，以便 Claude Code 理解您的代码库。组织可以在多个级别部署 CLAUDE.md 文件：

* **组织范围**：部署到系统目录，如 `/Library/Application Support/ClaudeCode/CLAUDE.md`（macOS），用于公司范围的标准
* **存储库级别**：在存储库根目录中创建 `CLAUDE.md` 文件，包含项目架构、构建命令和贡献指南。将这些文件检入源代码控制，以便所有用户受益

  [了解更多](/zh-CN/memory)。

### 2. 简化部署

如果您有自定义开发环境，我们发现创建一种"一键"安装 Claude Code 的方式是在组织中推动采用的关键。

### 3.从引导式使用开始

鼓励新用户尝试 Claude Code 进行代码库问答，或在较小的错误修复或功能请求上使用。要求 Claude Code 制定计划。检查 Claude 的建议，如果偏离轨道，请提供反馈。随着时间的推移，当用户更好地理解这种新范式时，他们将更有效地让 Claude Code 更自主地运行。

### 4. 配置安全策略

安全团队可以配置托管权限，以确定 Claude Code 允许和不允许做什么，这不能被本地配置覆盖。[了解更多](/zh-CN/security)。

### 5. 利用 MCP 进行集成

MCP 是为 Claude Code 提供更多信息的好方法，例如连接到票证管理系统或错误日志。我们建议一个中央团队配置 MCP 服务器，并将 `.mcp.json` 配置检入代码库，以便所有用户受益。[了解更多](/zh-CN/mcp)。

在 Anthropic，我们信任 Claude Code 为每个 Anthropic 代码库的开发提供动力。我们希望您像我们一样享受使用 Claude Code。

## 后续步骤

* [设置 Amazon Bedrock](/zh-CN/amazon-bedrock) 用于 AWS 原生部署
* [配置 Google Vertex AI](/zh-CN/google-vertex-ai) 用于 GCP 部署
* [设置 Microsoft Foundry](/zh-CN/microsoft-foundry) 用于 Azure 部署
* [配置企业网络](/zh-CN/network-config) 用于网络需求
* [部署 LLM 网关](/zh-CN/llm-gateway) 用于企业管理
* [设置](/zh-CN/settings) 用于配置选项和环境变量
