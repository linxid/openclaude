---
title: "优化您的终端设置"
description: "Claude Code 在终端配置正确时效果最佳。请按照这些指南来优化您的体验。"
---

### 主题和外观

Claude 无法控制您的终端主题。这由您的终端应用程序处理。您可以随时通过 `/config` 命令将 Claude Code 的主题与您的终端相匹配。

为了进一步自定义 Claude Code 界面本身，您可以配置一个[自定义状态行](/zh-CN/statusline)来显示上下文信息，如当前模型、工作目录或 git 分支在您的终端底部。

### 换行符

您有多个选项可以在 Claude Code 中输入换行符：

* **快速转义**：输入 `\` 后按 Enter 键创建新行
* **Shift+Enter**：在 iTerm2、WezTerm、Ghostty 和 Kitty 中开箱即用
* **键盘快捷键**：在其他终端中设置键绑定以插入新行

**为其他终端设置 Shift+Enter**

在 Claude Code 中运行 `/terminal-setup` 以自动为 VS Code、Alacritty、Zed 和 Warp 配置 Shift+Enter。

<Note>
  `/terminal-setup` 命令仅在需要手动配置的终端中可见。如果您使用的是 iTerm2、WezTerm、Ghostty 或 Kitty，您将看不到此命令，因为 Shift+Enter 已经原生工作。
</Note>

**设置 Option+Enter（VS Code、iTerm2 或 macOS Terminal.app）**

**对于 Mac Terminal.app：**

1. 打开设置 → 配置文件 → 键盘
2. 勾选"使用 Option 作为 Meta 键"

**对于 iTerm2 和 VS Code 终端：**

1. 打开设置 → 配置文件 → 键
2. 在常规下，将左/右 Option 键设置为"Esc+"

### 通知设置

通过正确的通知配置，永远不会错过 Claude 完成任务的时刻：

#### iTerm 2 系统通知

对于任务完成时的 iTerm 2 警报：

1. 打开 iTerm 2 偏好设置
2. 导航到配置文件 → 终端
3. 启用"静音铃声"和过滤警报 → "发送转义序列生成的警报"
4. 设置您首选的通知延迟

请注意，这些通知特定于 iTerm 2，在默认的 macOS 终端中不可用。

#### 自定义通知钩子

对于高级通知处理，您可以创建[通知钩子](/zh-CN/hooks#notification)来运行您自己的逻辑。

### 处理大型输入

处理大量代码或长指令时：

* **避免直接粘贴**：Claude Code 可能难以处理非常长的粘贴内容
* **使用基于文件的工作流**：将内容写入文件并要求 Claude 读取它
* **注意 VS Code 的限制**：VS Code 终端特别容易截断长粘贴

### Vim 模式

Claude Code 支持可以通过 `/vim` 启用或通过 `/config` 配置的 Vim 键绑定子集。

支持的子集包括：

* 模式切换：`Esc`（到 NORMAL）、`i`/`I`、`a`/`A`、`o`/`O`（到 INSERT）
* 导航：`h`/`j`/`k`/`l`、`w`/`e`/`b`、`0`/`$`/`^`、`gg`/`G`、`f`/`F`/`t`/`T` 带 `;`/`,` 重复
* 编辑：`x`、`dw`/`de`/`db`/`dd`/`D`、`cw`/`ce`/`cb`/`cc`/`C`、`.`（重复）
* 复制/粘贴：`yy`/`Y`、`yw`/`ye`/`yb`、`p`/`P`
* 文本对象：`iw`/`aw`、`iW`/`aW`、`i"`/`a"`、`i'`/`a'`、`i(`/`a(`、`i[`/`a[`、`i{`/`a{`
* 缩进：`>>`/`<<`
* 行操作：`J`（连接行）

请参阅[交互模式](/zh-CN/interactive-mode#vim-editor-mode)获取完整参考。
