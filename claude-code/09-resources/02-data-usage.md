---
title: "数据使用"
description: "了解 Anthropic 对 Claude 的数据使用政策"
---

# 数据使用

> 了解 Anthropic 对 Claude 的数据使用政策

## 数据政策

### 数据训练政策

**消费者用户（免费、Pro 和 Max 计划）**：
我们给您选择是否允许您的数据用于改进未来的 Claude 模型。当此设置打开时，我们将使用来自免费、Pro 和 Max 账户的数据来训练新模型（包括当您从这些账户使用 Claude Code 时）。

**商业用户**：（Team 和 Enterprise 计划、API、第三方平台和 Claude Gov）维持现有政策：Anthropic 不会在商业条款下使用发送到 Claude Code 的代码或提示来训练生成模型，除非客户选择向我们提供数据以改进模型（例如，[开发者合作伙伴计划](https://support.claude.com/en/articles/11174108-about-the-development-partner-program)）。

### 开发者合作伙伴计划

如果您明确选择加入向我们提供材料以进行训练的方法，例如通过[开发者合作伙伴计划](https://support.claude.com/en/articles/11174108-about-the-development-partner-program)，我们可能会使用所提供的材料来训练我们的模型。组织管理员可以明确选择为其组织加入开发者合作伙伴计划。请注意，此计划仅适用于 Anthropic 第一方 API，不适用于 Bedrock 或 Vertex 用户。

### 使用 `/bug` 命令的反馈

如果您选择使用 `/bug` 命令向我们发送有关 Claude Code 的反馈，我们可能会使用您的反馈来改进我们的产品和服务。通过 `/bug` 共享的记录将保留 5 年。

### 会话质量调查

当您在 Claude Code 中看到"Claude 在此会话中表现如何？"提示时，对此调查的回应（包括选择"关闭"）仅记录您的数字评分（1、2、3 或关闭）。我们不会作为此调查的一部分收集或存储任何对话记录、输入、输出或其他会话数据。与竖起大拇指/竖起大拇指向下反馈或 `/bug` 报告不同，此会话质量调查是一个简单的产品满意度指标。您对此调查的回应不会影响您的数据训练偏好，也不能用于训练我们的 AI 模型。

要禁用这些调查，请设置 `CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY=1`。当使用第三方提供商（Bedrock、Vertex、Foundry）或禁用遥测时，调查也会自动禁用。

### 数据保留

Anthropic 根据您的账户类型和偏好保留 Claude Code 数据。

**消费者用户（免费、Pro 和 Max 计划）**：

* 允许数据用于模型改进的用户：5 年保留期以支持模型开发和安全改进
* 不允许数据用于模型改进的用户：30 天保留期
* 隐私设置可以随时在 [claude.ai/settings/data-privacy-controls](https://claude.ai/settings/data-privacy-controls) 更改。

**商业用户（Team、Enterprise 和 API）**：

* 标准：30 天保留期
* 零数据保留：可通过适当配置的 API 密钥获得 - Claude Code 不会在服务器上保留聊天记录
* 本地缓存：Claude Code 客户端可能会在本地存储会话长达 30 天以启用会话恢复（可配置）

在我们的[隐私中心](https://privacy.anthropic.com/)了解更多关于数据保留实践的信息。

有关完整详情，请查看我们的[商业服务条款](https://www.anthropic.com/legal/commercial-terms)（适用于 Team、Enterprise 和 API 用户）或[消费者条款](https://www.anthropic.com/legal/consumer-terms)（适用于免费、Pro 和 Max 用户）和[隐私政策](https://www.anthropic.com/legal/privacy)。

## 数据访问

对于所有第一方用户，您可以了解更多关于为[本地 Claude Code](#local-claude-code-data-flow-and-dependencies) 和[远程 Claude Code](#cloud-execution-data-flow-and-dependencies) 记录的数据。请注意，对于远程 Claude Code，Claude 访问您启动 Claude Code 会话的存储库。Claude 不访问您已连接但未在其中启动会话的存储库。

## 本地 Claude Code：数据流和依赖项

下面的图表显示了 Claude Code 在安装和正常操作期间如何连接到外部服务。实线表示必需的连接，而虚线表示可选或用户启动的数据流。

<img src="https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=9e77f476347e7c9983f6e211d27cf6a9" alt="显示 Claude Code 外部连接的图表：安装/更新连接到 NPM，用户请求连接到 Anthropic 服务，包括 Console 身份验证、public-api，以及可选的 Statsig、Sentry 和错误报告" data-og-width="720" width="720" data-og-height="520" height="520" data-path="images/claude-code-data-flow.svg" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=280&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=94c033b9b6db3d10b9e2d7c6d681d9dc 280w, https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=560&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=430aaaf77c28c501d5753ffa456ee227 560w, https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=840&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=63c3c3f160b522220a8291fe2f93f970 840w, https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=1100&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=a7f6e838482f4a1a0a0b4683439369ea 1100w, https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=1650&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=5fbf749c2f94babb3ef72edfb7aba1e9 1650w, https://mintcdn.com/claude-code/I9Dpo7RZuIbc86cX/images/claude-code-data-flow.svg?w=2500&fit=max&auto=format&n=I9Dpo7RZuIbc86cX&q=85&s=7a1babbdccc4986957698d9c5c30c4a8 2500w" />

Claude Code 从 [NPM](https://www.npmjs.com/package/@anthropic-ai/claude-code) 安装。Claude Code 在本地运行。为了与 LLM 交互，Claude Code 通过网络发送数据。此数据包括所有用户提示和模型输出。数据通过 TLS 在传输中加密，在静止时未加密。Claude Code 与大多数流行的 VPN 和 LLM 代理兼容。

Claude Code 基于 Anthropic 的 API 构建。有关我们 API 的安全控制的详情，包括我们的 API 日志记录程序，请参考 [Anthropic 信任中心](https://trust.anthropic.com)中提供的合规工件。

### 云执行：数据流和依赖项

当使用[网络上的 Claude Code](/zh-CN/claude-code-on-the-web) 时，会话在 Anthropic 管理的虚拟机中运行，而不是在本地运行。在云环境中：

* \*\*代码和数据存储：\*\*您的存储库被克隆到隔离的 VM。代码和会话数据受您的账户类型的保留和使用政策约束（请参阅上面的数据保留部分）
* \*\*凭证：\*\*GitHub 身份验证通过安全代理处理；您的 GitHub 凭证永远不会进入沙箱
* \*\*网络流量：\*\*所有出站流量都通过安全代理进行审计日志记录和滥用防止
* \*\*会话数据：\*\*提示、代码更改和输出遵循与本地 Claude Code 使用相同的数据政策

有关云执行的安全详情，请参阅[安全](/zh-CN/security#cloud-execution-security)。

## 遥测服务

Claude Code 从用户的机器连接到 Statsig 服务以记录操作指标，例如延迟、可靠性和使用模式。此日志记录不包括任何代码或文件路径。数据使用 TLS 在传输中加密，使用 256 位 AES 加密在静止时加密。在 [Statsig 安全文档](https://www.statsig.com/trust/security)中了解更多。要选择退出 Statsig 遥测，请设置 `DISABLE_TELEMETRY` 环境变量。

Claude Code 从用户的机器连接到 Sentry 以进行操作错误日志记录。数据使用 TLS 在传输中加密，使用 256 位 AES 加密在静止时加密。在 [Sentry 安全文档](https://sentry.io/security/)中了解更多。要选择退出错误日志记录，请设置 `DISABLE_ERROR_REPORTING` 环境变量。

当用户运行 `/bug` 命令时，他们的完整对话历史记录（包括代码）的副本被发送到 Anthropic。数据在传输中和静止时加密。可选地，在我们的公共存储库中创建 Github 问题。要选择退出错误报告，请设置 `DISABLE_BUG_COMMAND` 环境变量。

## 按 API 提供商的默认行为

默认情况下，当使用 Bedrock、Vertex 或 Foundry 时，我们禁用所有非必需流量（包括错误报告、遥测、错误报告功能和会话质量调查）。您也可以通过设置 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 环境变量一次选择退出所有这些。以下是完整的默认行为：

| 服务                        | Claude API                                             | Vertex API                                 | Bedrock API                                 | Foundry API                                 |
| ------------------------- | ------------------------------------------------------ | ------------------------------------------ | ------------------------------------------- | ------------------------------------------- |
| **Statsig（指标）**           | 默认开启。<br />`DISABLE_TELEMETRY=1` 禁用。                   | 默认关闭。<br />`CLAUDE_CODE_USE_VERTEX` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_BEDROCK` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_FOUNDRY` 必须为 1。 |
| **Sentry（错误）**            | 默认开启。<br />`DISABLE_ERROR_REPORTING=1` 禁用。             | 默认关闭。<br />`CLAUDE_CODE_USE_VERTEX` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_BEDROCK` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_FOUNDRY` 必须为 1。 |
| **Claude API（`/bug` 报告）** | 默认开启。<br />`DISABLE_BUG_COMMAND=1` 禁用。                 | 默认关闭。<br />`CLAUDE_CODE_USE_VERTEX` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_BEDROCK` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_FOUNDRY` 必须为 1。 |
| **会话质量调查**                | 默认开启。<br />`CLAUDE_CODE_DISABLE_FEEDBACK_SURVEY=1` 禁用。 | 默认关闭。<br />`CLAUDE_CODE_USE_VERTEX` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_BEDROCK` 必须为 1。 | 默认关闭。<br />`CLAUDE_CODE_USE_FOUNDRY` 必须为 1。 |

所有环境变量都可以检入 `settings.json`（[了解更多](/zh-CN/settings)）。
