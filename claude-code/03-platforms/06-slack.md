---
title: "Slack 中的 Claude Code"
description: "直接从 Slack 工作区委派编码任务"
---

# Slack 中的 Claude Code

> 直接从 Slack 工作区委派编码任务

Slack 中的 Claude Code 将 Claude Code 的强大功能直接引入您的 Slack 工作区。当您使用编码任务提及 `@Claude` 时，Claude 会自动检测意图并在网络上创建 Claude Code 会话，允许您在不离开团队对话的情况下委派开发工作。

此集成基于现有的 Claude for Slack 应用程序构建，但为与编码相关的请求添加了到网络上 Claude Code 的智能路由。

## 用例

* **Bug 调查和修复**：要求 Claude 在 Slack 频道中报告 Bug 时立即调查和修复。
* **快速代码审查和修改**：让 Claude 根据团队反馈实现小功能或重构代码。
* **协作调试**：当团队讨论提供关键背景信息（例如错误重现或用户报告）时，Claude 可以使用该信息来指导其调试方法。
* **并行任务执行**：在 Slack 中启动编码任务，同时继续其他工作，完成时收到通知。

## 前置条件

在使用 Slack 中的 Claude Code 之前，请确保您具有以下条件：

| 要求               | 详情                                                        |
| :--------------- | :-------------------------------------------------------- |
| Claude 计划        | Pro、Max、Team 或 Enterprise，具有 Claude Code 访问权限（高级席位）       |
| 网络上的 Claude Code | 必须启用对[网络上的 Claude Code](/zh-CN/claude-code-on-the-web)的访问 |
| GitHub 账户        | 连接到网络上的 Claude Code，至少有一个存储库已认证                           |
| Slack 认证         | 您的 Slack 账户通过 Claude 应用程序链接到您的 Claude 账户                  |

## 在 Slack 中设置 Claude Code

<Steps>
  <Step title="在 Slack 中安装 Claude 应用程序">
    工作区管理员必须从 Slack 应用程序市场安装 Claude 应用程序。访问 [Slack 应用程序市场](https://slack.com/marketplace/A08SF47R6P4)并单击"Add to Slack"开始安装过程。
  </Step>

  <Step title="连接您的 Claude 账户">
    安装应用程序后，认证您的个人 Claude 账户：

    1. 通过单击您的应用程序部分中的"Claude"在 Slack 中打开 Claude 应用程序
    2. 导航到应用程序主页选项卡
    3. 单击"Connect"将您的 Slack 账户与您的 Claude 账户链接
    4. 在浏览器中完成认证流程
  </Step>

  <Step title="配置网络上的 Claude Code">
    确保您网络上的 Claude Code 已正确配置：

    * 访问 [claude.ai/code](https://claude.ai/code) 并使用您连接到 Slack 的同一账户登录
    * 如果尚未连接，请连接您的 GitHub 账户
    * 认证至少一个您希望 Claude 使用的存储库
  </Step>

  <Step title="选择您的路由模式">
    连接您的账户后，配置 Claude 如何在 Slack 中处理您的消息。导航到 Slack 中的 Claude 应用程序主页以找到**路由模式**设置。

    | 模式          | 行为                                                                                                    |
    | :---------- | :---------------------------------------------------------------------------------------------------- |
    | **仅代码**     | Claude 将所有 @mentions 路由到 Claude Code 会话。最适合仅将 Claude 用于 Slack 中开发任务的团队。                               |
    | **代码 + 聊天** | Claude 分析每条消息并在 Claude Code（用于编码任务）和 Claude Chat（用于写作、分析和常见问题）之间智能路由。最适合希望为所有类型工作提供单一 @Claude 入口点的团队。 |

    <Note>
      在代码 + 聊天模式下，如果 Claude 将消息路由到聊天但您想要编码会话，您可以单击"Retry as Code"来创建 Claude Code 会话。类似地，如果它被路由到代码但您想要聊天会话，您可以在该线程中选择该选项。
    </Note>
  </Step>
</Steps>

## 工作原理

### 自动检测

当您在 Slack 频道或线程中提及 @Claude 时，Claude 会自动分析您的消息以确定它是否是编码任务。如果 Claude 检测到编码意图，它将把您的请求路由到网络上的 Claude Code，而不是作为常规聊天助手响应。

您也可以明确告诉 Claude 将请求作为编码任务处理，即使它没有自动检测到。

<Note>
  Slack 中的 Claude Code 仅在频道（公开或私有）中工作。它在直接消息 (DM) 中不起作用。
</Note>

### 上下文收集

**来自线程**：当您在线程中 @mention Claude 时，它会从该线程中的所有消息收集上下文以理解完整的对话。

**来自频道**：当直接在频道中提及时，Claude 会查看最近的频道消息以获取相关上下文。

此上下文帮助 Claude 理解问题、选择适当的存储库并指导其任务方法。

<Warning>
  当在 Slack 中调用 @Claude 时，Claude 可以访问对话上下文以更好地理解您的请求。Claude 可能会遵循上下文中其他消息的指示，因此用户应确保仅在受信任的 Slack 对话中使用 Claude。
</Warning>

### 会话流程

1. **启动**：您使用编码请求 @mention Claude
2. **检测**：Claude 分析您的消息并检测编码意图
3. **会话创建**：在 claude.ai/code 上创建新的 Claude Code 会话
4. **进度更新**：Claude 在工作进行时向您的 Slack 线程发布状态更新
5. **完成**：完成后，Claude @mentions 您并提供摘要和操作按钮
6. **审查**：单击"View Session"查看完整记录，或单击"Create PR"打开拉取请求

## 用户界面元素

### 应用程序主页

应用程序主页选项卡显示您的连接状态，并允许您连接或断开您的 Claude 账户与 Slack 的连接。

### 消息操作

* **View Session**：在浏览器中打开完整的 Claude Code 会话，您可以在其中查看所有执行的工作、继续会话或提出其他请求。
* **Create PR**：直接从会话的更改创建拉取请求。
* **Retry as Code**：如果 Claude 最初作为聊天助手响应但您想要编码会话，请单击此按钮将请求重试为 Claude Code 任务。
* **Change Repo**：如果 Claude 选择不正确，允许您选择不同的存储库。

### 存储库选择

Claude 根据 Slack 对话中的上下文自动选择存储库。如果多个存储库可能适用，Claude 可能会显示一个下拉菜单，允许您选择正确的存储库。

## 访问和权限

### 用户级访问

| 访问类型           | 要求                                        |
| :------------- | :---------------------------------------- |
| Claude Code 会话 | 每个用户在其自己的 Claude 账户下运行会话                  |
| 使用情况和速率限制      | 会话计入个人用户的计划限制                             |
| 存储库访问          | 用户只能访问他们个人连接的存储库                          |
| 会话历史           | 会话出现在您在 claude.ai/code 上的 Claude Code 历史中 |

### 工作区管理员权限

Slack 工作区管理员控制 Claude 应用程序是否可以在工作区中安装。然后，各个用户使用自己的 Claude 账户进行认证以使用集成。

## 什么可以在哪里访问

**在 Slack 中**：您将看到状态更新、完成摘要和操作按钮。完整记录被保留并始终可访问。

**在网络上**：完整的 Claude Code 会话，包含完整的对话历史、所有代码更改、文件操作以及继续会话或创建拉取请求的能力。

## 最佳实践

### 编写有效的请求

* **具体说明**：在相关时包括文件名、函数名或错误消息。
* **提供上下文**：如果从对话中不清楚，请提及存储库或项目。
* **定义成功**：解释"完成"的样子——Claude 应该编写测试吗？更新文档？创建 PR？
* **使用线程**：在讨论 Bug 或功能时在线程中回复，以便 Claude 可以收集完整的上下文。

### 何时使用 Slack 与网络

**在以下情况下使用 Slack**：上下文已存在于 Slack 讨论中，您想异步启动任务，或者您正在与需要可见性的团队成员协作。

**直接在网络上使用**：当您需要上传文件、想要在开发过程中进行实时交互或处理更长、更复杂的任务时。

## 故障排除

### 会话未启动

1. 验证您的 Claude 账户在 Claude 应用程序主页中已连接
2. 检查您是否启用了网络上的 Claude Code 访问权限
3. 确保您至少有一个 GitHub 存储库连接到 Claude Code

### 存储库未显示

1. 在 [claude.ai/code](https://claude.ai/code) 的网络上的 Claude Code 中连接存储库
2. 验证您对该存储库的 GitHub 权限
3. 尝试断开并重新连接您的 GitHub 账户

### 选择了错误的存储库

1. 单击"Change Repo"按钮选择不同的存储库
2. 在您的请求中包括存储库名称以获得更准确的选择

### 认证错误

1. 在应用程序主页中断开并重新连接您的 Claude 账户
2. 确保您在浏览器中登录到正确的 Claude 账户
3. 检查您的 Claude 计划是否包括 Claude Code 访问权限

### 会话过期

1. 会话在网络上的 Claude Code 历史中保持可访问
2. 您可以从 [claude.ai/code](https://claude.ai/code) 继续或参考过去的会话

## 当前限制

* **仅 GitHub**：目前支持 GitHub 上的存储库。
* **一次一个 PR**：每个会话可以创建一个拉取请求。
* **速率限制适用**：会话使用您的个人 Claude 计划的速率限制。
* **需要网络访问**：用户必须具有网络上的 Claude Code 访问权限；没有它的用户将只获得标准 Claude 聊天响应。

## 相关资源

<CardGroup>
  <Card title="网络上的 Claude Code" icon="globe" href="/zh-CN/claude-code-on-the-web">
    了解有关网络上的 Claude Code 的更多信息
  </Card>

  <Card title="Claude for Slack" icon="slack" href="https://claude.com/claude-and-slack">
    Claude for Slack 常规文档
  </Card>

  <Card title="Slack 应用程序市场" icon="store" href="https://slack.com/marketplace/A08SF47R6P4">
    从 Slack 市场安装 Claude 应用程序
  </Card>

  <Card title="Claude 帮助中心" icon="circle-question" href="https://support.claude.com">
    获取额外支持
  </Card>
</CardGroup>
