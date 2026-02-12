---
title: "快速入门"
description: "欢迎使用 Claude Code！"
---

本快速入门指南将在几分钟内让您使用 AI 驱动的编码辅助。完成后，您将了解如何使用 Claude Code 完成常见的开发任务。

## 开始前

确保您拥有：

- 打开的终端或命令提示符
- 一个要处理的代码项目
- 一个 [Claude.ai](https://claude.ai)（推荐）或 [Claude Console](https://console.anthropic.com/) 账户

## 步骤 1：安装 Claude Code

To install Claude Code, use one of the following methods:

<Tabs>
  <Tab title="Native Install (Recommended)">
    **macOS, Linux, WSL:**

    ```bash
    curl -fsSL https://claude.ai/install.sh | bash
    ```

    **Windows PowerShell:**

    ```powershell
    irm https://claude.ai/install.ps1 | iex
    ```

    **Windows CMD:**

    ```batch
    curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
    ```

    <Info>
      Native installations automatically update in the background to keep you on the latest version.
    </Info>
  </Tab>
  <Tab title="Homebrew">
    ```sh
    brew install --cask claude-code
    ```

    <Info>
      Homebrew installations do not auto-update. Run `brew upgrade claude-code` periodically to get the latest features and security fixes.
    </Info>
  </Tab>
  <Tab title="WinGet">
    ```powershell
    winget install Anthropic.ClaudeCode
    ```

    <Info>
      WinGet installations do not auto-update. Run `winget upgrade Anthropic.ClaudeCode` periodically to get the latest features and security fixes.
    </Info>
  </Tab>
</Tabs>

## 步骤 2：登录您的账户

Claude Code 需要账户才能使用。当您使用 `claude` 命令启动交互式会话时，您需要登录：

```bash
claude
# 首次使用时系统会提示您登录
```

```bash
/login
# 按照提示使用您的账户登录
```

您可以使用以下任一账户类型登录：

- [Claude.ai](https://claude.ai)（订阅计划 - 推荐）
- [Claude Console](https://console.anthropic.com/)（使用预付费额度的 API 访问）

登录后，您的凭证将被存储，您无需再次登录。

<Note>
  当您首次使用 Claude Console 账户对 Claude Code 进行身份验证时，系统会自动为您创建一个名为"Claude Code"的工作区。此工作区为您的组织中所有 Claude Code 使用情况提供集中的成本跟踪和管理。
</Note>

<Note>
  您可以在同一电子邮件地址下拥有两种账户类型。如果您需要再次登录或切换账户，请在 Claude Code 中使用 `/login` 命令。
</Note>

<Tip>
  打开 Claude 后，有多种方式可以使用 Claude，默认的方式是登录 Claude 官方账号，如何配置第三方 API，可以参阅 [第三方 API](/claude-code/01-getting-started/04-third-party-api)
</Tip>

## 步骤 3：启动您的第一个会话

在任何项目目录中打开您的终端并启动 Claude Code：

```bash
cd /path/to/your/project
claude
```

您将看到 Claude Code 欢迎屏幕，其中包含您的会话信息、最近的对话和最新更新。输入 `/help` 查看可用命令或输入 `/resume` 继续之前的对话。

<Tip>
  登录后（步骤 2），您的凭证将存储在您的系统上。在 [凭证管理](/claude-code/07-configuration/02-permissions#credential-management) 中了解更多信息。
</Tip>

## 步骤 4：提出您的第一个问题

让我们从了解您的代码库开始。尝试以下命令之一：

```
> what does this project do?
```

Claude 将分析您的文件并提供摘要。您也可以提出更具体的问题：

```
> what technologies does this project use?
```

```
> where is the main entry point?
```

```
> explain the folder structure
```

您也可以询问 Claude 它自己的功能：

```
> what can Claude Code do?
```

```
> how do I use slash commands in Claude Code?
```

```
> can Claude Code work with Docker?
```

<Note>
  Claude Code 根据需要读取您的文件 - 您无需手动添加上下文。Claude 还可以访问自己的文档，并可以回答有关其功能和特性的问题。
</Note>

## 步骤 5：进行您的第一次代码更改

现在让我们让 Claude Code 进行一些实际的编码。尝试一个简单的任务：

```
> add a hello world function to the main file
```

Claude Code 将：

1. 找到合适的文件
2. 向您显示建议的更改
3. 请求您的批准
4. 进行编辑

<Note>
  Claude Code 在修改文件前始终请求权限。您可以批准单个更改或为会话启用"全部接受"模式。
</Note>

## 步骤 6：在 Claude Code 中使用 Git

Claude Code 使 Git 操作变得对话式：

```
> what files have I changed?
```

```
> commit my changes with a descriptive message
```

您也可以提示更复杂的 Git 操作：

```
> create a new branch called feature/quickstart
```

```
> show me the last 5 commits
```

```
> help me resolve merge conflicts
```

## 步骤 7：修复错误或添加功能

Claude 擅长调试和功能实现。

用自然语言描述您想要的内容：

```
> add input validation to the user registration form
```

或修复现有问题：

```
> there's a bug where users can submit empty forms - fix it
```

Claude Code 将：

- 定位相关代码
- 理解上下文
- 实现解决方案
- 如果可用，运行测试

## 步骤 8：尝试其他常见工作流

有许多方式可以与 Claude 合作：

**重构代码**

```
> refactor the authentication module to use async/await instead of callbacks
```

**编写测试**

```
> write unit tests for the calculator functions
```

**更新文档**

```
> update the README with installation instructions
```

**代码审查**

```
> review my changes and suggest improvements
```

<Tip>
  **记住**：Claude Code 是您的 AI 配对程序员。像与有帮助的同事交谈一样与它交谈 - 描述您想要实现的目标，它将帮助您实现。
</Tip>

## 基本命令

以下是日常使用中最重要的命令：

| 命令                  | 功能             | 示例                                  |
| ------------------- | -------------- | ----------------------------------- |
| `claude`            | 启动交互模式         | `claude`                            |
| `claude "task"`     | 运行一次性任务        | `claude "fix the build error"`      |
| `claude -p "query"` | 运行一次性查询，然后退出   | `claude -p "explain this function"` |
| `claude -c`         | 在当前目录中继续最近的对话  | `claude -c`                         |
| `claude -r`         | 恢复之前的对话        | `claude -r`                         |
| `claude commit`     | 创建 Git 提交      | `claude commit`                     |
| `/clear`            | 清除对话历史         | `> /clear`                          |
| `/help`             | 显示可用命令         | `> /help`                           |
| `exit` 或 Ctrl+C     | 退出 Claude Code | `> exit`                            |

有关完整的命令列表，请参阅 [CLI 参考](/claude-code/08-reference/01-cli-reference)。

## 初学者专业提示

<AccordionGroup>
  <Accordion title="对您的请求要具体">
    不要这样说："fix the bug"

    尝试这样说："fix the login bug where users see a blank screen after entering wrong credentials"
  </Accordion>
  <Accordion title="使用分步说明">
    将复杂任务分解为步骤：

    ```
    > 1. create a new database table for user profiles
    ```

    ```
    > 2. create an API endpoint to get and update user profiles
    ```

    ```
    > 3. build a webpage that allows users to see and edit their information
    ```
  </Accordion>
  <Accordion title="让 Claude 先探索">
    在进行更改之前，让 Claude 了解您的代码：

    ```
    > analyze the database schema
    ```

    ```
    > build a dashboard showing products that are most frequently returned by our UK customers
    ```
  </Accordion>
  <Accordion title="使用快捷方式节省时间">
    - 按 `?` 查看所有可用的键盘快捷方式
    - 使用 Tab 进行命令补全
    - 按 ↑ 查看命令历史
    - 输入 `/` 查看所有斜杠命令
  </Accordion>
</AccordionGroup>

## 接下来呢？

现在您已经学习了基础知识，探索更多高级功能：

<CardGroup cols={3}>
  <Card title="常见工作流" icon="graduation-cap" href="/claude-code/02-core-concepts/03-common-workflows">
    常见任务的分步指南
  </Card>
  <Card title="CLI 参考" icon="terminal" href="/claude-code/08-reference/01-cli-reference">
    掌握所有命令和选项
  </Card>
  <Card title="配置" icon="gear" href="/claude-code/07-configuration/01-settings">
    为您的工作流自定义 Claude Code
  </Card>
  <Card title="网络上的 Claude Code" icon="cloud" href="/claude-code/03-platforms/01-claude-code-on-the-web">
    在云中异步运行任务
  </Card>
  <Card title="关于 Claude Code" icon="sparkles" href="https://claude.com/product/claude-code">
    在 claude.com 上了解更多
  </Card>
</CardGroup>

## 获取帮助

- **在 Claude Code 中**：输入 `/help` 或询问"how do I..."
- **文档**：您在这里！浏览其他指南
- **社区**：加入我们的 [Discord](https://www.anthropic.com/discord) 获取提示和支持