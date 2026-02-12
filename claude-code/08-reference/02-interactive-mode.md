---
title: "交互模式"
description: "Claude Code 会话中键盘快捷键、输入模式和交互功能的完整参考。"
---

## 键盘快捷键

<Note>
  键盘快捷键可能因平台和终端而异。按 `?` 查看您的环境中可用的快捷键。

  **macOS 用户**：Option/Alt 键快捷键（`Alt+B`、`Alt+F`、`Alt+Y`、`Alt+M`、`Alt+P`）需要在终端中将 Option 配置为 Meta：

  * **iTerm2**：设置 → 配置文件 → 键 → 将左/右 Option 键设置为"Esc+"
  * **Terminal.app**：设置 → 配置文件 → 键盘 → 勾选"使用 Option 作为 Meta 键"
  * **VS Code**：设置 → 配置文件 → 键 → 将左/右 Option 键设置为"Esc+"

  有关详细信息，请参阅[终端配置](/claude-code/07-configuration/06-terminal-config)。
</Note>

### 常规控制

| 快捷键                                          | 描述                | 上下文                                        |
| :------------------------------------------- | :---------------- | :----------------------------------------- |
| `Ctrl+C`                                     | 取消当前输入或生成         | 标准中断                                       |
| `Ctrl+D`                                     | 退出 Claude Code 会话 | EOF 信号                                     |
| `Ctrl+G`                                     | 在默认文本编辑器中打开       | 在默认文本编辑器中编辑您的提示或自定义响应                      |
| `Ctrl+L`                                     | 清除终端屏幕            | 保留对话历史                                     |
| `Ctrl+O`                                     | 切换详细输出            | 显示详细的工具使用和执行情况                             |
| `Ctrl+R`                                     | 反向搜索命令历史          | 交互式搜索以前的命令                                 |
| `Ctrl+V` 或 `Cmd+V`（iTerm2）或 `Alt+V`（Windows） | 从剪贴板粘贴图像          | 粘贴图像或图像文件的路径                               |
| `Ctrl+B`                                     | 后台运行任务            | 后台运行 bash 命令和代理。Tmux 用户按两次                 |
| `Left/Right arrows`                          | 在对话框选项卡之间循环       | 在权限对话框和菜单中的选项卡之间导航                         |
| `Up/Down arrows`                             | 导航命令历史            | 回忆以前的输入                                    |
| `Esc` + `Esc`                                | 回退代码/对话           | 将代码和/或对话恢复到之前的状态                           |
| `Shift+Tab` 或 `Alt+M`（某些配置）                  | 切换权限模式            | 在自动接受模式、Plan Mode 和正常模式之间切换                |
| `Option+P`（macOS）或 `Alt+P`（Windows/Linux）    | 切换模型              | 在不清除提示的情况下切换模型                             |
| `Option+T`（macOS）或 `Alt+T`（Windows/Linux）    | 切换扩展思考            | 启用或禁用扩展思考模式。首先运行 `/terminal-setup` 以启用此快捷键 |

### 文本编辑

| 快捷键                    | 描述          | 上下文                                                                 |
| :--------------------- | :---------- | :------------------------------------------------------------------ |
| `Ctrl+K`               | 删除到行尾       | 存储已删除的文本以供粘贴                                                        |
| `Ctrl+U`               | 删除整行        | 存储已删除的文本以供粘贴                                                        |
| `Ctrl+Y`               | 粘贴已删除的文本    | 粘贴用 `Ctrl+K` 或 `Ctrl+U` 删除的文本                                       |
| `Alt+Y`（在 `Ctrl+Y` 之后） | 循环粘贴历史      | 粘贴后，循环浏览以前删除的文本。在 macOS 上需要[将 Option 设置为 Meta](#keyboard-shortcuts) |
| `Alt+B`                | 将光标向后移动一个单词 | 单词导航。在 macOS 上需要[将 Option 设置为 Meta](#keyboard-shortcuts)            |
| `Alt+F`                | 将光标向前移动一个单词 | 单词导航。在 macOS 上需要[将 Option 设置为 Meta](#keyboard-shortcuts)            |

### 主题和显示

| 快捷键      | 描述         | 上下文                                           |
| :------- | :--------- | :-------------------------------------------- |
| `Ctrl+T` | 切换代码块的语法高亮 | 仅在 `/theme` 选择器菜单内工作。控制 Claude 响应中的代码是否使用语法着色 |

<Note>
  语法高亮仅在 Claude Code 的原生构建中可用。
</Note>

### 多行输入

| 方法          | 快捷键            | 上下文                                  |
| :---------- | :------------- | :----------------------------------- |
| 快速转义        | `\` + `Enter`  | 在所有终端中工作                             |
| macOS 默认    | `Option+Enter` | macOS 上的默认设置                         |
| Shift+Enter | `Shift+Enter`  | 在 iTerm2、WezTerm、Ghostty、Kitty 中开箱即用 |
| 控制序列        | `Ctrl+J`       | 多行的换行符                               |
| 粘贴模式        | 直接粘贴           | 对于代码块、日志                             |

<Tip>
  Shift+Enter 在 iTerm2、WezTerm、Ghostty 和 Kitty 中无需配置即可工作。对于其他终端（VS Code、Alacritty、Zed、Warp），运行 `/terminal-setup` 以安装绑定。
</Tip>

### 快速命令

| 快捷键     | 描述        | 注释                                                     |
| :------ | :-------- | :----------------------------------------------------- |
| `/` 在开始 | 命令或 skill | 请参阅[内置命令](#built-in-commands)和 [skills](/claude-code/04-build-with-claude/01-skills) |
| `!` 在开始 | Bash 模式   | 直接运行命令并将执行输出添加到会话                                      |
| `@`     | 文件路径提及    | 触发文件路径自动完成                                             |

## 内置命令

内置命令是常见操作的快捷方式。下表涵盖常用命令，但不是所有可用选项。在 Claude Code 中键入 `/` 以查看完整列表，或键入 `/` 后跟任何字母以进行筛选。

要创建您可以使用 `/` 调用的自己的命令，请参阅 [skills](/claude-code/04-build-with-claude/01-skills)。

| 命令                        | 目的                                                                        |
| :------------------------ | :------------------------------------------------------------------------ |
| `/clear`                  | 清除对话历史                                                                    |
| `/compact [instructions]` | 使用可选焦点指令压缩对话                                                              |
| `/config`                 | 打开设置界面（配置选项卡）                                                             |
| `/context`                | 将当前上下文使用情况可视化为彩色网格                                                        |
| `/cost`                   | 显示令牌使用统计信息。有关特定于订阅的详细信息，请参阅[成本跟踪指南](/claude-code/06-administration/08-costs#using-the-cost-command)。 |
| `/doctor`                 | 检查您的 Claude Code 安装的健康状况                                                  |
| `/exit`                   | 退出 REPL                                                                   |
| `/export [filename]`      | 将当前对话导出到文件或剪贴板                                                            |
| `/help`                   | 获取使用帮助                                                                    |
| `/init`                   | 使用 `CLAUDE.md` 指南初始化项目                                                    |
| `/mcp`                    | 管理 MCP server 连接和 OAuth 身份验证                                              |
| `/memory`                 | 编辑 `CLAUDE.md` 内存文件                                                       |
| `/model`                  | 选择或更改 AI 模型                                                               |
| `/permissions`            | 查看或更新[权限](/claude-code/07-configuration/02-permissions#configuring-permissions)                             |
| `/plan`                   | 直接从提示进入 Plan Mode                                                         |
| `/rename <name>`          | 重命名当前会话以便于识别                                                              |
| `/resume [session]`       | 按 ID 或名称恢复对话，或打开会话选择器                                                     |
| `/rewind`                 | 回退对话和/或代码                                                                 |
| `/stats`                  | 可视化每日使用情况、会话历史、连胜和模型偏好                                                    |
| `/status`                 | 打开设置界面（状态选项卡），显示版本、模型、帐户和连接性                                              |
| `/statusline`             | 设置 Claude Code 的状态行 UI                                                    |
| `/tasks`                  | 列出并管理后台任务                                                                 |
| `/teleport`               | 从 claude.ai 恢复远程会话（仅限订阅者）                                                 |
| `/theme`                  | 更改颜色主题                                                                    |
| `/todos`                  | 列出当前 TODO 项                                                               |
| `/usage`                  | 仅适用于订阅计划：显示计划使用限制和速率限制状态                                                  |

### MCP prompts

MCP servers 可以公开显示为命令的 prompts。这些使用格式 `/mcp__<server>__<prompt>`，并从连接的服务器动态发现。有关详细信息，请参阅 [MCP prompts](/claude-code/04-build-with-claude/05-mcp#use-mcp-prompts-as-commands)。

## Vim 编辑器模式

使用 `/vim` 命令启用 vim 风格编辑，或通过 `/config` 永久配置。

### 模式切换

| 命令    | 操作           | 来自模式   |
| :---- | :----------- | :----- |
| `Esc` | 进入 NORMAL 模式 | INSERT |
| `i`   | 在光标前插入       | NORMAL |
| `I`   | 在行首插入        | NORMAL |
| `a`   | 在光标后插入       | NORMAL |
| `A`   | 在行尾插入        | NORMAL |
| `o`   | 在下方打开行       | NORMAL |
| `O`   | 在上方打开行       | NORMAL |

### 导航（NORMAL 模式）

| 命令              | 操作                  |
| :-------------- | :------------------ |
| `h`/`j`/`k`/`l` | 向左/下/上/右移动          |
| `w`             | 下一个单词               |
| `e`             | 单词末尾                |
| `b`             | 上一个单词               |
| `0`             | 行首                  |
| `$`             | 行尾                  |
| `^`             | 第一个非空白字符            |
| `gg`            | 输入开始                |
| `G`             | 输入结束                |
| `f{char}`       | 跳转到下一个字符出现处         |
| `F{char}`       | 跳转到上一个字符出现处         |
| `t{char}`       | 跳转到下一个字符出现处之前       |
| `T{char}`       | 跳转到上一个字符出现处之后       |
| `;`             | 重复最后一个 f/F/t/T 动作   |
| `,`             | 反向重复最后一个 f/F/t/T 动作 |

### 编辑（NORMAL 模式）

| 命令             | 操作          |
| :------------- | :---------- |
| `x`            | 删除字符        |
| `dd`           | 删除行         |
| `D`            | 删除到行尾       |
| `dw`/`de`/`db` | 删除单词/到末尾/向后 |
| `cc`           | 更改行         |
| `C`            | 更改到行尾       |
| `cw`/`ce`/`cb` | 更改单词/到末尾/向后 |
| `yy`/`Y`       | 复制（yank）行   |
| `yw`/`ye`/`yb` | 复制单词/到末尾/向后 |
| `p`            | 在光标后粘贴      |
| `P`            | 在光标前粘贴      |
| `>>`           | 缩进行         |
| `<<`           | 取消缩进行       |
| `J`            | 连接行         |
| `.`            | 重复最后一个更改    |

### 文本对象（NORMAL 模式）

文本对象与 `d`、`c` 和 `y` 等运算符一起工作：

| 命令        | 操作               |
| :-------- | :--------------- |
| `iw`/`aw` | 内部/周围单词          |
| `iW`/`aW` | 内部/周围 WORD（空格分隔） |
| `i"`/`a"` | 内部/周围双引号         |
| `i'`/`a'` | 内部/周围单引号         |
| `i(`/`a(` | 内部/周围括号          |
| `i[`/`a[` | 内部/周围方括号         |
| `i{`/`a{` | 内部/周围花括号         |

## 命令历史

Claude Code 为当前会话维护命令历史：

* 历史按工作目录存储
* 使用 `/clear` 命令清除
* 使用上/下箭头导航（请参阅上面的键盘快捷键）
* **注意**：历史扩展（`!`）默认禁用

### 使用 Ctrl+R 反向搜索

按 `Ctrl+R` 交互式搜索您的命令历史：

1. **开始搜索**：按 `Ctrl+R` 激活反向历史搜索
2. **键入查询**：输入文本以在以前的命令中搜索 - 搜索词将在匹配结果中突出显示
3. **导航匹配**：再次按 `Ctrl+R` 循环浏览较旧的匹配
4. **接受匹配**：
   * 按 `Tab` 或 `Esc` 接受当前匹配并继续编辑
   * 按 `Enter` 接受并立即执行命令
5. **取消搜索**：
   * 按 `Ctrl+C` 取消并恢复原始输入
   * 在空搜索上按 `Backspace` 取消

搜索显示匹配的命令，搜索词突出显示，使您可以轻松找到并重用以前的输入。

## 后台 bash 命令

Claude Code 支持在后台运行 bash 命令，允许您在长时间运行的进程执行时继续工作。

### 后台运行的工作原理

当 Claude Code 在后台运行命令时，它异步运行命令并立即返回后台任务 ID。Claude Code 可以在命令继续在后台执行时响应新提示。

要在后台运行命令，您可以：

* 提示 Claude Code 在后台运行命令
* 按 Ctrl+B 将常规 Bash 工具调用移到后台。（Tmux 用户必须按两次 Ctrl+B，因为 tmux 的前缀键。）

**主要功能：**

* 输出被缓冲，Claude 可以使用 TaskOutput 工具检索它
* 后台任务具有用于跟踪和输出检索的唯一 ID
* 当 Claude Code 退出时，后台任务会自动清理

要禁用所有后台任务功能，请将 `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS` 环境变量设置为 `1`。有关详细信息，请参阅[环境变量](/claude-code/07-configuration/01-settings#environment-variables)。

**常见后台命令：**

* 构建工具（webpack、vite、make）
* 包管理器（npm、yarn、pnpm）
* 测试运行器（jest、pytest）
* 开发服务器
* 长时间运行的进程（docker、terraform）

### 使用 `!` 前缀的 Bash 模式

通过在输入前加上 `!` 直接运行 bash 命令，无需通过 Claude：

```bash  theme={null}
! npm test
! git status
! ls -la
```

Bash 模式：

* 将命令及其输出添加到对话上下文
* 显示实时进度和输出
* 支持相同的 `Ctrl+B` 后台运行长时间运行的命令
* 不需要 Claude 解释或批准命令
* 支持基于历史的自动完成：键入部分命令并按 **Tab** 从当前项目中的以前的 `!` 命令完成

这对于快速 shell 操作同时保持对话上下文很有用。

## 任务列表

在处理复杂的多步工作时，Claude 创建任务列表来跟踪进度。任务出现在终端的状态区域中，指示器显示待处理、进行中或完成的内容。

* 按 `Ctrl+T` 切换任务列表视图。显示一次最多 10 个任务
* 要查看所有任务或清除它们，直接询问 Claude："show me all tasks"（显示所有任务）或"clear all tasks"（清除所有任务）
* 任务在上下文压缩中持续存在，帮助 Claude 在较大的项目中保持组织
* 要在会话之间共享任务列表，请设置 `CLAUDE_CODE_TASK_LIST_ID` 以使用 `~/.claude/tasks/` 中的命名目录：`CLAUDE_CODE_TASK_LIST_ID=my-project claude`

## 另请参阅

* [Skills](/claude-code/04-build-with-claude/01-skills) - 自定义 prompts 和工作流
* [Checkpointing](/claude-code/08-reference/03-checkpointing) - 回退 Claude 的编辑并恢复以前的状态
* [CLI 参考](/claude-code/08-reference/01-cli-reference) - 命令行标志和选项
* [设置](/claude-code/07-configuration/01-settings) - 配置选项
* [内存管理](/claude-code/07-configuration/03-memory) - 管理 CLAUDE.md 文件
