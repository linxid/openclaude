---
title: "JetBrains IDEs"
description: "在 JetBrains IDEs（包括 IntelliJ、PyCharm、WebStorm 等）中使用 Claude Code"
---

# JetBrains IDEs

> 在 JetBrains IDEs（包括 IntelliJ、PyCharm、WebStorm 等）中使用 Claude Code

Claude Code 通过专用插件与 JetBrains IDEs 集成，提供交互式差异查看、选择上下文共享等功能。

## 支持的 IDEs

Claude Code 插件适用于大多数 JetBrains IDEs，包括：

* IntelliJ IDEA
* PyCharm
* Android Studio
* WebStorm
* PhpStorm
* GoLand

## 功能

* **快速启动**：使用 `Cmd+Esc`（Mac）或 `Ctrl+Esc`（Windows/Linux）直接从编辑器打开 Claude Code，或点击 UI 中的 Claude Code 按钮
* **差异查看**：代码更改可以直接在 IDE 差异查看器中显示，而不是在终端中显示
* **选择上下文**：IDE 中的当前选择/标签页会自动与 Claude Code 共享
* **文件引用快捷方式**：使用 `Cmd+Option+K`（Mac）或 `Alt+Ctrl+K`（Linux/Windows）插入文件引用（例如，@File#L1-99）
* **诊断共享**：IDE 中的诊断错误（lint、语法等）在您工作时会自动与 Claude 共享

## 安装

### 市场安装

从 JetBrains 市场查找并安装 [Claude Code 插件](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-)，然后重启您的 IDE。

如果您还没有安装 Claude Code，请参阅[我们的快速入门指南](/zh-CN/quickstart)了解安装说明。

<Note>
  安装插件后，您可能需要完全重启 IDE 才能使其生效。
</Note>

## 使用

### 从您的 IDE

从 IDE 的集成终端运行 `claude`，所有集成功能将处于活跃状态。

### 从外部终端

在任何外部终端中使用 `/ide` 命令将 Claude Code 连接到您的 JetBrains IDE 并激活所有功能：

```bash  theme={null}
claude
> /ide
```

如果您希望 Claude 能够访问与 IDE 相同的文件，请从与 IDE 项目根目录相同的目录启动 Claude Code。

## 配置

### Claude Code 设置

通过 Claude Code 的设置配置 IDE 集成：

1. 运行 `claude`
2. 输入 `/config` 命令
3. 将差异工具设置为 `auto` 以进行自动 IDE 检测

### 插件设置

通过转到 **Settings → Tools → Claude Code \[Beta]** 配置 Claude Code 插件：

#### 常规设置

* **Claude 命令**：指定自定义命令来运行 Claude（例如，`claude`、`/usr/local/bin/claude` 或 `npx @anthropic/claude`）
* **抑制 Claude 命令未找到的通知**：跳过关于找不到 Claude 命令的通知
* **启用使用 Option+Enter 进行多行提示**（仅限 macOS）：启用后，Option+Enter 在 Claude Code 提示中插入新行。如果遇到 Option 键被意外捕获的问题，请禁用此选项（需要重启终端）
* **启用自动更新**：自动检查并安装插件更新（在重启时应用）

<Tip>
  对于 WSL 用户：将 `wsl -d Ubuntu -- bash -lic "claude"` 设置为您的 Claude 命令（将 `Ubuntu` 替换为您的 WSL 发行版名称）
</Tip>

#### ESC 键配置

如果 ESC 键不能中断 JetBrains 终端中的 Claude Code 操作：

1. 转到 **Settings → Tools → Terminal**
2. 执行以下任一操作：
   * 取消选中"使用 Escape 将焦点移动到编辑器"，或
   * 点击"配置终端键绑定"并删除"切换焦点到编辑器"快捷方式
3. 应用更改

这将允许 ESC 键正确中断 Claude Code 操作。

## 特殊配置

### 远程开发

<Warning>
  使用 JetBrains 远程开发时，您必须通过 **Settings → Plugin (Host)** 在远程主机上安装插件。
</Warning>

插件必须安装在远程主机上，而不是在您的本地客户端计算机上。

### WSL 配置

<Warning>
  WSL 用户可能需要额外配置才能使 IDE 检测正常工作。有关详细的设置说明，请参阅我们的 [WSL 故障排除指南](/zh-CN/troubleshooting#jetbrains-ide-not-detected-on-wsl2)。
</Warning>

WSL 配置可能需要：

* 正确的终端配置
* 网络模式调整
* 防火墙设置更新

## 故障排除

### 插件不工作

* 确保您从项目根目录运行 Claude Code
* 检查 JetBrains 插件在 IDE 设置中是否启用
* 完全重启 IDE（您可能需要多次执行此操作）
* 对于远程开发，确保插件已安装在远程主机上

### IDE 未检测到

* 验证插件已安装并启用
* 完全重启 IDE
* 检查您是否从集成终端运行 Claude Code
* 对于 WSL 用户，请参阅 [WSL 故障排除指南](/zh-CN/troubleshooting#jetbrains-ide-not-detected-on-wsl2)

### 命令未找到

如果点击 Claude 图标显示"命令未找到"：

1. 验证 Claude Code 已安装：`npm list -g @anthropic-ai/claude-code`
2. 在插件设置中配置 Claude 命令路径
3. 对于 WSL 用户，使用配置部分中提到的 WSL 命令格式

## 安全考虑

当 Claude Code 在启用自动编辑权限的 JetBrains IDE 中运行时，它可能能够修改可由您的 IDE 自动执行的 IDE 配置文件。这可能会增加在自动编辑模式下运行 Claude Code 的风险，并允许绕过 Claude Code 的 bash 执行权限提示。

在 JetBrains IDEs 中运行时，请考虑：

* 对编辑使用手动批准模式
* 特别小心确保 Claude 仅与受信任的提示一起使用
* 了解 Claude Code 有权访问修改的文件

如需更多帮助，请参阅我们的[故障排除指南](/zh-CN/troubleshooting)。
