---
title: "常见工作流程"
description: "使用 Claude Code 探索代码库、修复错误、重构、测试和其他日常任务的分步指南。"
---

# 常见工作流程

> 使用 Claude Code 探索代码库、修复错误、重构、测试和其他日常任务的分步指南。

本页涵盖日常开发的实用工作流程：探索陌生代码、调试、重构、编写测试、创建 PR 和管理会话。每个部分都包含示例提示，您可以根据自己的项目进行调整。有关更高级的模式和提示，请参阅[最佳实践](/zh-CN/best-practices)。

## 理解新代码库

### 快速获取代码库概览

假设您刚加入一个新项目，需要快速了解其结构。

<Steps>
  <Step title="导航到项目根目录">
    ```bash  theme={null}
    cd /path/to/project 
    ```
  </Step>

  <Step title="启动 Claude Code">
    ```bash  theme={null}
    claude 
    ```
  </Step>

  <Step title="请求高级概览">
    ```
    > give me an overview of this codebase 
    ```
  </Step>

  <Step title="深入了解特定组件">
    ```
    > explain the main architecture patterns used here 
    ```

    ```
    > what are the key data models?
    ```

    ```
    > how is authentication handled?
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 从广泛的问题开始，然后缩小到特定领域
  * 询问项目中使用的编码约定和模式
  * 请求项目特定术语的词汇表
</Tip>

### 查找相关代码

假设您需要定位与特定功能相关的代码。

<Steps>
  <Step title="要求 Claude 查找相关文件">
    ```
    > find the files that handle user authentication 
    ```
  </Step>

  <Step title="获取有关组件如何交互的上下文">
    ```
    > how do these authentication files work together? 
    ```
  </Step>

  <Step title="理解执行流程">
    ```
    > trace the login process from front-end to database 
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 明确说明您要查找的内容
  * 使用项目中的领域语言
  * 为您的语言安装[代码智能插件](/zh-CN/discover-plugins#code-intelligence)，以便为 Claude 提供精确的"转到定义"和"查找引用"导航
</Tip>

***

## 高效修复错误

假设您遇到了错误消息，需要找到并修复其来源。

<Steps>
  <Step title="与 Claude 分享错误">
    ```
    > I'm seeing an error when I run npm test 
    ```
  </Step>

  <Step title="请求修复建议">
    ```
    > suggest a few ways to fix the @ts-ignore in user.ts 
    ```
  </Step>

  <Step title="应用修复">
    ```
    > update user.ts to add the null check you suggested 
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 告诉 Claude 重现问题的命令并获取堆栈跟踪
  * 提及重现错误的任何步骤
  * 让 Claude 知道错误是间歇性的还是持续的
</Tip>

***

## 重构代码

假设您需要更新旧代码以使用现代模式和实践。

<Steps>
  <Step title="识别用于重构的遗留代码">
    ```
    > find deprecated API usage in our codebase 
    ```
  </Step>

  <Step title="获取重构建议">
    ```
    > suggest how to refactor utils.js to use modern JavaScript features 
    ```
  </Step>

  <Step title="安全地应用更改">
    ```
    > refactor utils.js to use ES2024 features while maintaining the same behavior 
    ```
  </Step>

  <Step title="验证重构">
    ```
    > run tests for the refactored code 
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 要求 Claude 解释现代方法的优势
  * 请求在需要时保持向后兼容性的更改
  * 以小的、可测试的增量进行重构
</Tip>

***

## 使用专门的 subagents

假设您想使用专门的 AI subagents 来更有效地处理特定任务。

<Steps>
  <Step title="查看可用的 subagents">
    ```
    > /agents
    ```

    这显示所有可用的 subagents 并让您创建新的。
  </Step>

  <Step title="自动使用 subagents">
    Claude Code 自动将适当的任务委派给专门的 subagents：

    ```
    > review my recent code changes for security issues
    ```

    ```
    > run all tests and fix any failures
    ```
  </Step>

  <Step title="明确请求特定的 subagents">
    ```
    > use the code-reviewer subagent to check the auth module
    ```

    ```
    > have the debugger subagent investigate why users can't log in
    ```
  </Step>

  <Step title="为您的工作流程创建自定义 subagents">
    ```
    > /agents
    ```

    然后选择"Create New subagent"并按照提示定义：

    * 描述 subagent 目的的唯一标识符（例如，`code-reviewer`、`api-designer`）。
    * Claude 何时应使用此 agent
    * 它可以访问哪些工具
    * 描述 agent 角色和行为的系统提示
  </Step>
</Steps>

<Tip>
  提示：

  * 在 `.claude/agents/` 中创建项目特定的 subagents 以供团队共享
  * 使用描述性的 `description` 字段来启用自动委派
  * 限制工具访问权限为每个 subagent 实际需要的内容
  * 查看[subagents 文档](/zh-CN/sub-agents)了解详细示例
</Tip>

***

## 使用 Plan Mode 进行安全的代码分析

Plan Mode 指示 Claude 通过使用只读操作分析代码库来创建计划，非常适合探索代码库、规划复杂更改或安全地审查代码。在 Plan Mode 中，Claude 使用 [`AskUserQuestion`](/zh-CN/settings#tools-available-to-claude) 来收集需求并在提出计划之前澄清您的目标。

### 何时使用 Plan Mode

* **多步骤实现**：当您的功能需要编辑许多文件时
* **代码探索**：当您想在更改任何内容之前彻底研究代码库时
* **交互式开发**：当您想与 Claude 迭代方向时

### 如何使用 Plan Mode

**在会话期间打开 Plan Mode**

您可以在会话期间使用 **Shift+Tab** 循环切换权限模式来切换到 Plan Mode。

如果您处于 Normal Mode，**Shift+Tab** 首先切换到 Auto-Accept Mode，在终端底部显示 `⏵⏵ accept edits on`。随后的 **Shift+Tab** 将切换到 Plan Mode，显示 `⏸ plan mode on`。当[agent team](/zh-CN/agent-teams)处于活动状态时，循环还包括 Delegate Mode。

**在 Plan Mode 中启动新会话**

要在 Plan Mode 中启动新会话，请使用 `--permission-mode plan` 标志：

```bash  theme={null}
claude --permission-mode plan
```

**在 Plan Mode 中运行"无头"查询**

您也可以直接在 Plan Mode 中使用 `-p` 运行查询（即在["无头模式"](/zh-CN/headless)中）：

```bash  theme={null}
claude --permission-mode plan -p "Analyze the authentication system and suggest improvements"
```

### 示例：规划复杂的重构

```bash  theme={null}
claude --permission-mode plan
```

```
> I need to refactor our authentication system to use OAuth2. Create a detailed migration plan.
```

Claude 分析当前实现并创建全面的计划。通过后续问题进行细化：

```
> What about backward compatibility?
> How should we handle database migration?
```

<Tip>按 `Ctrl+G` 在默认文本编辑器中打开计划，您可以在 Claude 继续之前直接编辑它。</Tip>

### 将 Plan Mode 配置为默认值

```json  theme={null}
// .claude/settings.json
{
  "permissions": {
    "defaultMode": "plan"
  }
}
```

有关更多配置选项，请参阅[设置文档](/zh-CN/settings#available-settings)。

***

## 使用测试

假设您需要为未覆盖的代码添加测试。

<Steps>
  <Step title="识别未测试的代码">
    ```
    > find functions in NotificationsService.swift that are not covered by tests 
    ```
  </Step>

  <Step title="生成测试框架">
    ```
    > add tests for the notification service 
    ```
  </Step>

  <Step title="添加有意义的测试用例">
    ```
    > add test cases for edge conditions in the notification service 
    ```
  </Step>

  <Step title="运行并验证测试">
    ```
    > run the new tests and fix any failures 
    ```
  </Step>
</Steps>

Claude 可以生成遵循您项目现有模式和约定的测试。请求测试时，请明确说明您想验证的行为。Claude 检查您现有的测试文件以匹配已在使用的样式、框架和断言模式。

为了获得全面的覆盖，要求 Claude 识别您可能遗漏的边界情况。Claude 可以分析您的代码路径并建议测试错误条件、边界值和容易被忽视的意外输入。

***

## 创建 pull requests

您可以直接要求 Claude 创建 pull requests（"create a pr for my changes"）或使用 `/commit-push-pr` skill，它在一个步骤中提交、推送和打开 PR。

```
> /commit-push-pr
```

如果您配置了 Slack MCP server 并在 CLAUDE.md 中指定了频道（例如，"post PR URLs to #team-prs"），该 skill 会自动将 PR URL 发布到这些频道。

为了更好地控制流程，逐步指导 Claude 完成或[创建您自己的 skill](/zh-CN/skills)：

<Steps>
  <Step title="总结您的更改">
    ```
    > summarize the changes I've made to the authentication module
    ```
  </Step>

  <Step title="生成 pull request">
    ```
    > create a pr
    ```
  </Step>

  <Step title="审查和细化">
    ```
    > enhance the PR description with more context about the security improvements
    ```
  </Step>
</Steps>

当您使用 `gh pr create` 创建 PR 时，会话会自动链接到该 PR。您可以稍后使用 `claude --from-pr <number>` 恢复它。

<Tip>
  在提交前审查 Claude 生成的 PR，并要求 Claude 突出显示潜在的风险或注意事项。
</Tip>

## 处理文档

假设您需要为代码添加或更新文档。

<Steps>
  <Step title="识别未记录的代码">
    ```
    > find functions without proper JSDoc comments in the auth module 
    ```
  </Step>

  <Step title="生成文档">
    ```
    > add JSDoc comments to the undocumented functions in auth.js 
    ```
  </Step>

  <Step title="审查和增强">
    ```
    > improve the generated documentation with more context and examples 
    ```
  </Step>

  <Step title="验证文档">
    ```
    > check if the documentation follows our project standards 
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 指定您想要的文档样式（JSDoc、docstrings 等）
  * 要求文档中包含示例
  * 请求公共 API、接口和复杂逻辑的文档
</Tip>

***

## 使用图像

假设您需要在代码库中使用图像，并希望 Claude 帮助分析图像内容。

<Steps>
  <Step title="将图像添加到对话中">
    您可以使用以下任何方法：

    1. 将图像拖放到 Claude Code 窗口中
    2. 复制图像并使用 ctrl+v 将其粘贴到 CLI 中（不要使用 cmd+v）
    3. 向 Claude 提供图像路径。例如，"Analyze this image: /path/to/your/image.png"
  </Step>

  <Step title="要求 Claude 分析图像">
    ```
    > What does this image show?
    ```

    ```
    > Describe the UI elements in this screenshot
    ```

    ```
    > Are there any problematic elements in this diagram?
    ```
  </Step>

  <Step title="使用图像获取上下文">
    ```
    > Here's a screenshot of the error. What's causing it?
    ```

    ```
    > This is our current database schema. How should we modify it for the new feature?
    ```
  </Step>

  <Step title="从视觉内容获取代码建议">
    ```
    > Generate CSS to match this design mockup
    ```

    ```
    > What HTML structure would recreate this component?
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 当文本描述不清楚或繁琐时使用图像
  * 包含错误、UI 设计或图表的屏幕截图以获得更好的上下文
  * 您可以在对话中使用多个图像
  * 图像分析适用于图表、屏幕截图、模型等
  * 当 Claude 引用图像时（例如，`[Image #1]`），`Cmd+Click`（Mac）或 `Ctrl+Click`（Windows/Linux）链接以在默认查看器中打开图像
</Tip>

***

## 引用文件和目录

使用 @ 快速包含文件或目录，无需等待 Claude 读取它们。

<Steps>
  <Step title="引用单个文件">
    ```
    > Explain the logic in @src/utils/auth.js
    ```

    这在对话中包含文件的完整内容。
  </Step>

  <Step title="引用目录">
    ```
    > What's the structure of @src/components?
    ```

    这提供了带有文件信息的目录列表。
  </Step>

  <Step title="引用 MCP 资源">
    ```
    > Show me the data from @github:repos/owner/repo/issues
    ```

    这使用 @server:resource 格式从连接的 MCP servers 获取数据。有关详细信息，请参阅 [MCP 资源](/zh-CN/mcp#use-mcp-resources)。
  </Step>
</Steps>

<Tip>
  提示：

  * 文件路径可以是相对的或绝对的
  * @ 文件引用在文件的目录和父目录中添加 `CLAUDE.md` 到上下文
  * 目录引用显示文件列表，而不是内容
  * 您可以在单个消息中引用多个文件（例如，"@file1.js and @file2.js"）
</Tip>

***

## 使用扩展思考（Thinking Mode）

[扩展思考](https://platform.claude.com/docs/en/build-with-claude/extended-thinking)默认启用，为 Claude 提供空间在响应前逐步推理复杂问题。这种推理在详细模式中可见，您可以使用 `Ctrl+O` 切换。

此外，Opus 4.6 引入了自适应推理：不是固定的思考令牌预算，而是模型根据您的[努力级别](/zh-CN/model-config#adjust-effort-level)设置动态分配思考。扩展思考和自适应推理一起工作，让您可以控制 Claude 在响应前的推理深度。

扩展思考对于复杂的架构决策、具有挑战性的错误、多步骤实现规划和评估不同方法之间的权衡特别有价值。

<Note>
  "think"、"think hard"、"ultrathink" 和 "think more" 等短语被解释为常规提示指令，不分配思考令牌。
</Note>

### 配置 Thinking Mode

默认启用思考，但您可以调整或禁用它。

| 范围         | 如何配置                                                                                  | 详细信息                                                                             |
| ---------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **努力级别**   | 在 `/model` 中调整或设置 [`CLAUDE_CODE_EFFORT_LEVEL`](/zh-CN/settings#environment-variables) | 控制 Opus 4.6 的思考深度：低、中、高（默认）。请参阅[调整努力级别](/zh-CN/model-config#adjust-effort-level) |
| **切换快捷键**  | 按 `Option+T`（macOS）或 `Alt+T`（Windows/Linux）                                           | 为当前会话切换思考开/关（所有模型）。可能需要[终端配置](/zh-CN/terminal-config)来启用 Option 键快捷键             |
| **全局默认值**  | 使用 `/config` 切换 Thinking Mode                                                         | 在所有项目中设置默认值（所有模型）。<br />保存为 `~/.claude/settings.json` 中的 `alwaysThinkingEnabled` |
| **限制令牌预算** | 设置 [`MAX_THINKING_TOKENS`](/zh-CN/settings#environment-variables) 环境变量                | 将思考预算限制为特定数量的令牌（在 Opus 4.6 上忽略，除非设置为 0）。示例：`export MAX_THINKING_TOKENS=10000`    |

要查看 Claude 的思考过程，按 `Ctrl+O` 切换详细模式，并查看显示为灰色斜体文本的内部推理。

### 扩展思考如何工作

扩展思考控制 Claude 在响应前执行多少内部推理。更多思考提供更多空间来探索解决方案、分析边界情况和自我纠正错误。

**使用 Opus 4.6**，思考使用自适应推理：模型根据您选择的[努力级别](/zh-CN/model-config#adjust-effort-level)（低、中、高）动态分配思考令牌。这是调整速度和推理深度之间权衡的推荐方式。

**使用其他模型**，思考使用固定预算，最多 31,999 个令牌来自您的输出预算。您可以使用 [`MAX_THINKING_TOKENS`](/zh-CN/settings#environment-variables) 环境变量限制此，或通过 `/config` 或 `Option+T`/`Alt+T` 切换完全禁用思考。

当使用 Opus 4.6 时，`MAX_THINKING_TOKENS` 被忽略，因为自适应推理控制思考深度。唯一的例外：设置 `MAX_THINKING_TOKENS=0` 仍然在任何模型上完全禁用思考。

<Warning>
  您需要为所有使用的思考令牌付费，即使 Claude 4 模型显示总结的思考
</Warning>

***

## 恢复以前的对话

启动 Claude Code 时，您可以恢复以前的会话：

* `claude --continue` 继续当前目录中最近的对话
* `claude --resume` 打开对话选择器或按名称恢复
* `claude --from-pr 123` 恢复链接到特定 pull request 的会话

从活动会话内，使用 `/resume` 切换到不同的对话。

会话按项目目录存储。`/resume` 选择器显示来自同一 git 存储库的会话，包括 worktrees。

### 命名您的会话

给会话起描述性名称以便稍后找到它们。这是在处理多个任务或功能时的最佳实践。

<Steps>
  <Step title="命名当前会话">
    在会话期间使用 `/rename` 给它一个易记的名称：

    ```
    > /rename auth-refactor
    ```

    您也可以从选择器重命名任何会话：运行 `/resume`，导航到会话，然后按 `R`。
  </Step>

  <Step title="稍后按名称恢复">
    从命令行：

    ```bash  theme={null}
    claude --resume auth-refactor
    ```

    或从活动会话内：

    ```
    > /resume auth-refactor
    ```
  </Step>
</Steps>

### 使用会话选择器

`/resume` 命令（或 `claude --resume` 不带参数）打开具有以下功能的交互式会话选择器：

**选择器中的键盘快捷键：**

| 快捷键       | 操作               |
| :-------- | :--------------- |
| `↑` / `↓` | 在会话之间导航          |
| `→` / `←` | 展开或折叠分组的会话       |
| `Enter`   | 选择并恢复突出显示的会话     |
| `P`       | 预览会话内容           |
| `R`       | 重命名突出显示的会话       |
| `/`       | 搜索以过滤会话          |
| `A`       | 在当前目录和所有项目之间切换   |
| `B`       | 过滤来自当前 git 分支的会话 |
| `Esc`     | 退出选择器或搜索模式       |

**会话组织：**

选择器显示带有有用元数据的会话：

* 会话名称或初始提示
* 自上次活动以来经过的时间
* 消息计数
* Git 分支（如果适用）

分叉的会话（使用 `/rewind` 或 `--fork-session` 创建）在其根会话下分组，使查找相关对话更容易。

<Tip>
  提示：

  * **尽早命名会话**：在开始处理不同任务时使用 `/rename`——稍后找到"payment-integration"比"explain this function"容易得多
  * 使用 `--continue` 快速访问当前目录中最近的对话
  * 当您知道需要哪个会话时使用 `--resume session-name`
  * 当您需要浏览和选择时使用 `--resume`（不带名称）
  * 对于脚本，使用 `claude --continue --print "prompt"` 以非交互模式恢复
  * 在选择器中按 `P` 在恢复前预览会话
  * 恢复的对话以与原始对话相同的模型和配置开始

  工作原理：

  1. **对话存储**：所有对话都自动保存在本地，包含完整的消息历史
  2. **消息反序列化**：恢复时，整个消息历史被恢复以保持上下文
  3. **工具状态**：来自以前对话的工具使用和结果被保留
  4. **上下文恢复**：对话以所有以前的上下文完整恢复
</Tip>

***

## 使用 Git worktrees 运行并行 Claude Code 会话

假设您需要同时处理多个任务，并在 Claude Code 实例之间完全隔离代码。

<Steps>
  <Step title="理解 Git worktrees">
    Git worktrees 允许您从同一存储库中检出多个分支到单独的目录中。每个 worktree 都有自己的工作目录，文件隔离，同时共享相同的 Git 历史。在[官方 Git worktree 文档](https://git-scm.com/docs/git-worktree)中了解更多。
  </Step>

  <Step title="创建新的 worktree">
    ```bash  theme={null}
    # 使用新分支创建新的 worktree 
    git worktree add ../project-feature-a -b feature-a

    # 或使用现有分支创建 worktree
    git worktree add ../project-bugfix bugfix-123
    ```

    这创建了一个新目录，其中包含存储库的单独工作副本。
  </Step>

  <Step title="在每个 worktree 中运行 Claude Code">
    ```bash  theme={null}
    # 导航到您的 worktree 
    cd ../project-feature-a

    # 在此隔离环境中运行 Claude Code
    claude
    ```
  </Step>

  <Step title="在另一个 worktree 中运行 Claude">
    ```bash  theme={null}
    cd ../project-bugfix
    claude
    ```
  </Step>

  <Step title="管理您的 worktrees">
    ```bash  theme={null}
    # 列出所有 worktrees
    git worktree list

    # 完成后删除 worktree
    git worktree remove ../project-feature-a
    ```
  </Step>
</Steps>

<Tip>
  提示：

  * 每个 worktree 都有自己的独立文件状态，非常适合并行 Claude Code 会话
  * 在一个 worktree 中所做的更改不会影响其他 worktree，防止 Claude 实例相互干扰
  * 所有 worktrees 共享相同的 Git 历史和远程连接
  * 对于长期运行的任务，您可以在一个 worktree 中让 Claude 工作，同时在另一个 worktree 中继续开发
  * 使用描述性目录名称来轻松识别每个 worktree 的任务
  * 记住根据您的项目设置在每个新 worktree 中初始化您的开发环境。根据您的堆栈，这可能包括：
    * JavaScript 项目：运行依赖项安装（`npm install`、`yarn`）
    * Python 项目：设置虚拟环境或使用包管理器安装
    * 其他语言：遵循您项目的标准设置流程
</Tip>

有关具有共享任务和消息的并行会话的自动协调，请参阅 [agent teams](/zh-CN/agent-teams)。

***

## 将 Claude 用作 unix 风格的实用程序

### 将 Claude 添加到您的验证流程

假设您想使用 Claude Code 作为 linter 或代码审查工具。

**将 Claude 添加到您的构建脚本：**

```json  theme={null}
// package.json
{
    ...
    "scripts": {
        ...
        "lint:claude": "claude -p 'you are a linter. please look at the changes vs. main and report any issues related to typos. report the filename and line number on one line, and a description of the issue on the second line. do not return any other text.'"
    }
}
```

<Tip>
  提示：

  * 在 CI/CD 管道中使用 Claude 进行自动代码审查
  * 自定义提示以检查与您的项目相关的特定问题
  * 考虑为不同类型的验证创建多个脚本
</Tip>

### 管道输入、管道输出

假设您想将数据管道输入 Claude，并以结构化格式获取数据。

**通过 Claude 管道数据：**

```bash  theme={null}
cat build-error.txt | claude -p 'concisely explain the root cause of this build error' > output.txt
```

<Tip>
  提示：

  * 使用管道将 Claude 集成到现有 shell 脚本中
  * 与其他 Unix 工具结合以实现强大的工作流程
  * 考虑使用 --output-format 获得结构化输出
</Tip>

### 控制输出格式

假设您需要 Claude 的输出采用特定格式，特别是在将 Claude Code 集成到脚本或其他工具时。

<Steps>
  <Step title="使用文本格式（默认）">
    ```bash  theme={null}
    cat data.txt | claude -p 'summarize this data' --output-format text > summary.txt
    ```

    这仅输出 Claude 的纯文本响应（默认行为）。
  </Step>

  <Step title="使用 JSON 格式">
    ```bash  theme={null}
    cat code.py | claude -p 'analyze this code for bugs' --output-format json > analysis.json
    ```

    这输出包含元数据（包括成本和持续时间）的消息的 JSON 数组。
  </Step>

  <Step title="使用流式 JSON 格式">
    ```bash  theme={null}
    cat log.txt | claude -p 'parse this log file for errors' --output-format stream-json
    ```

    这在 Claude 处理请求时实时输出一系列 JSON 对象。每条消息都是有效的 JSON 对象，但如果连接，整个输出不是有效的 JSON。
  </Step>
</Steps>

<Tip>
  提示：

  * 对于简单集成（您只需要 Claude 的响应），使用 `--output-format text`
  * 当您需要完整的对话日志时使用 `--output-format json`
  * 对于每个对话轮次的实时输出，使用 `--output-format stream-json`
</Tip>

***

## 询问 Claude 其功能

Claude 内置访问其文档，可以回答有关其自身功能和限制的问题。

### 示例问题

```
> can Claude Code create pull requests?
```

```
> how does Claude Code handle permissions?
```

```
> what skills are available?
```

```
> how do I use MCP with Claude Code?
```

```
> how do I configure Claude Code for Amazon Bedrock?
```

```
> what are the limitations of Claude Code?
```

<Note>
  Claude 基于文档为这些问题提供答案。有关可执行示例和实际演示，请参阅上面的特定工作流程部分。
</Note>

<Tip>
  提示：

  * Claude 始终可以访问最新的 Claude Code 文档，无论您使用的版本如何
  * 提出具体问题以获得详细答案
  * Claude 可以解释复杂功能，如 MCP 集成、企业配置和高级工作流程
</Tip>

***

## 后续步骤

<CardGroup cols={2}>
  <Card title="最佳实践" icon="lightbulb" href="/zh-CN/best-practices">
    从 Claude Code 中获得最大收益的模式
  </Card>

  <Card title="Claude Code 如何工作" icon="gear" href="/zh-CN/how-claude-code-works">
    理解 agentic 循环和上下文管理
  </Card>

  <Card title="扩展 Claude Code" icon="puzzle-piece" href="/zh-CN/features-overview">
    添加 skills、hooks、MCP、subagents 和 plugins
  </Card>

  <Card title="参考实现" icon="code" href="https://github.com/anthropics/claude-code/tree/main/.devcontainer">
    克隆我们的开发容器参考实现
  </Card>
</CardGroup>
