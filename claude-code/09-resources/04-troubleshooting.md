---
title: "故障排除"
description: "发现 Claude Code 安装和使用中常见问题的解决方案。"
---

## 常见安装问题

### Windows 安装问题：WSL 中的错误

您可能会在 WSL 中遇到以下问题：

**操作系统/平台检测问题**：如果您在安装期间收到错误，WSL 可能正在使用 Windows `npm`。请尝试：

* 在安装前运行 `npm config set os linux`
* 使用 `npm install -g @anthropic-ai/claude-code --force --no-os-check` 安装（不要使用 `sudo`）

**找不到 Node 错误**：如果在运行 `claude` 时看到 `exec: node: not found`，您的 WSL 环境可能正在使用 Windows 安装的 Node.js。您可以使用 `which npm` 和 `which node` 确认这一点，它们应该指向以 `/usr/` 开头的 Linux 路径，而不是 `/mnt/c/`。要解决此问题，请尝试通过您的 Linux 发行版的包管理器或通过 [`nvm`](https://github.com/nvm-sh/nvm) 安装 Node。

**nvm 版本冲突**：如果您在 WSL 和 Windows 中都安装了 nvm，在 WSL 中切换 Node 版本时可能会遇到版本冲突。这是因为 WSL 默认导入 Windows PATH，导致 Windows nvm/npm 优先于 WSL 安装。

您可以通过以下方式识别此问题：

* 运行 `which npm` 和 `which node` - 如果它们指向 Windows 路径（以 `/mnt/c/` 开头），则正在使用 Windows 版本
* 在 WSL 中使用 nvm 切换 Node 版本后功能损坏

要解决此问题，请修复您的 Linux PATH 以确保 Linux node/npm 版本优先：

**主要解决方案：确保 nvm 在您的 shell 中正确加载**

最常见的原因是 nvm 未在非交互式 shell 中加载。将以下内容添加到您的 shell 配置文件（`~/.bashrc`、`~/.zshrc` 等）：

```bash  theme={null}
# Load nvm if it exists
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

或在您的当前会话中直接运行：

```bash  theme={null}
source ~/.nvm/nvm.sh
```

**替代方案：调整 PATH 顺序**

如果 nvm 已正确加载但 Windows 路径仍然优先，您可以在 shell 配置中显式地将 Linux 路径添加到 PATH 的前面：

```bash  theme={null}
export PATH="$HOME/.nvm/versions/node/$(node -v)/bin:$PATH"
```

<Warning>
  避免禁用 Windows PATH 导入（`appendWindowsPath = false`），因为这会破坏从 WSL 调用 Windows 可执行文件的能力。同样，如果您在 Windows 开发中使用 Node.js，请避免从 Windows 卸载它。
</Warning>

### Linux 和 Mac 安装问题：权限或找不到命令错误

使用 npm 安装 Claude Code 时，`PATH` 问题可能会阻止访问 `claude`。
如果您的 npm 全局前缀不可由用户写入（例如 `/usr` 或 `/usr/local`），您也可能会遇到权限错误。

#### 推荐解决方案：原生 Claude Code 安装

Claude Code 有一个不依赖 npm 或 Node.js 的原生安装。

使用以下命令运行原生安装程序。

**macOS、Linux、WSL：**

```bash  theme={null}
# Install stable version (default)
curl -fsSL https://claude.ai/install.sh | bash

# Install latest version
curl -fsSL https://claude.ai/install.sh | bash -s latest

# Install specific version number
curl -fsSL https://claude.ai/install.sh | bash -s 1.0.58
```

**Windows PowerShell：**

```powershell  theme={null}
# Install stable version (default)
irm https://claude.ai/install.ps1 | iex

# Install latest version
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) latest

# Install specific version number
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) 1.0.58

```

此命令为您的操作系统和架构安装适当的 Claude Code 构建，并在 `~/.local/bin/claude`（或 Windows 上的 `%USERPROFILE%\.local\bin\claude.exe`）添加安装的符号链接。

<Tip>
  确保您的系统 PATH 中有安装目录。
</Tip>

### Windows："Claude Code on Windows requires git-bash"

Windows 上的原生 Claude Code 需要 [Git for Windows](https://git-scm.com/downloads/win)，其中包括 Git Bash。如果已安装 Git 但未被检测到：

1. 在运行 Claude 之前在 PowerShell 中显式设置路径：
   ```powershell  theme={null}
   $env:CLAUDE_CODE_GIT_BASH_PATH="C:\Program Files\Git\bin\bash.exe"
   ```

2. 或通过系统属性 → 环境变量将其永久添加到系统环境变量中。

如果 Git 安装在非标准位置，请相应地调整路径。

### Windows："installMethod is native, but claude command not found"

如果在安装后看到此错误，`claude` 命令不在您的 PATH 中。手动添加它：

<Steps>
  <Step title="打开环境变量">
    按 `Win + R`，输入 `sysdm.cpl`，然后按 Enter。单击**高级** → **环境变量**。
  </Step>

  <Step title="编辑用户 PATH">
    在"用户变量"下，选择**路径**并单击**编辑**。单击**新建**并添加：

    ```
    %USERPROFILE%\.local\bin
    ```
  </Step>

  <Step title="重启您的终端">
    关闭并重新打开 PowerShell 或 CMD 以使更改生效。
  </Step>
</Steps>

验证安装：

```bash  theme={null}
claude doctor # Check installation health
```

## 权限和身份验证

### 重复的权限提示

如果您发现自己反复批准相同的命令，您可以使用 `/permissions` 命令允许特定工具在不批准的情况下运行。请参阅[权限文档](/zh-CN/iam#configuring-permissions)。

### 身份验证问题

如果您遇到身份验证问题：

1. 运行 `/logout` 完全注销
2. 关闭 Claude Code
3. 使用 `claude` 重新启动并再次完成身份验证过程

如果问题仍然存在，请尝试：

```bash  theme={null}
rm -rf ~/.config/claude-code/auth.json
claude
```

这会删除您存储的身份验证信息并强制进行干净登录。

## 配置文件位置

Claude Code 在多个位置存储配置：

| 文件                            | 用途                                                 |
| :---------------------------- | :------------------------------------------------- |
| `~/.claude/settings.json`     | 用户设置（权限、钩子、模型覆盖）                                   |
| `.claude/settings.json`       | 项目设置（检入源代码控制）                                      |
| `.claude/settings.local.json` | 本地项目设置（未提交）                                        |
| `~/.claude.json`              | 全局状态（主题、OAuth、MCP 服务器、允许的工具）                       |
| `.mcp.json`                   | 项目 MCP 服务器（检入源代码控制）                                |
| `managed-settings.json`       | [托管设置](/zh-CN/settings#settings-files)             |
| `managed-mcp.json`            | [托管 MCP 服务器](/zh-CN/mcp#managed-mcp-configuration) |

在 Windows 上，`~` 指的是您的用户主目录，例如 `C:\Users\YourName`。

**托管文件位置：**

* macOS：`/Library/Application Support/ClaudeCode/`
* Linux/WSL：`/etc/claude-code/`
* Windows：`C:\Program Files\ClaudeCode\`

有关配置这些文件的详细信息，请参阅[设置](/zh-CN/settings)和 [MCP](/zh-CN/mcp)。

### 重置配置

要将 Claude Code 重置为默认设置，您可以删除配置文件：

```bash  theme={null}
# Reset all user settings and state
rm ~/.claude.json
rm -rf ~/.claude/

# Reset project-specific settings
rm -rf .claude/
rm .mcp.json
```

<Warning>
  这将删除您的所有设置、允许的工具、MCP 服务器配置和会话历史记录。
</Warning>

## 性能和稳定性

### 高 CPU 或内存使用率

Claude Code 设计用于与大多数开发环境配合使用，但在处理大型代码库时可能会消耗大量资源。如果您遇到性能问题：

1. 定期使用 `/compact` 来减少上下文大小
2. 在主要任务之间关闭并重新启动 Claude Code
3. 考虑将大型构建目录添加到您的 `.gitignore` 文件中

### 命令挂起或冻结

如果 Claude Code 似乎无响应：

1. 按 Ctrl+C 尝试取消当前操作
2. 如果无响应，您可能需要关闭终端并重新启动

### 搜索和发现问题

如果搜索工具、`@file` 提及、自定义代理和自定义斜杠命令不起作用，请安装系统 `ripgrep`：

```bash  theme={null}
# macOS (Homebrew)  
brew install ripgrep

# Windows (winget)
winget install BurntSushi.ripgrep.MSVC

# Ubuntu/Debian
sudo apt install ripgrep

# Alpine Linux
apk add ripgrep

# Arch Linux
pacman -S ripgrep
```

然后在您的[环境](/zh-CN/settings#environment-variables)中设置 `USE_BUILTIN_RIPGREP=0`。

### WSL 上的搜索结果缓慢或不完整

在 WSL 上[跨文件系统工作](https://learn.microsoft.com/en-us/windows/wsl/filesystems)时的磁盘读取性能损失可能导致在 WSL 上使用 Claude Code 时匹配数少于预期（但不是完全缺乏搜索功能）。

<Note>
  在这种情况下，`/doctor` 将显示搜索为正常。
</Note>

**解决方案：**

1. **提交更具体的搜索**：通过指定目录或文件类型来减少搜索的文件数量："在 auth-service 包中搜索 JWT 验证逻辑"或"在 JS 文件中查找 md5 哈希的使用"。

2. **将项目移到 Linux 文件系统**：如果可能，确保您的项目位于 Linux 文件系统（`/home/`）而不是 Windows 文件系统（`/mnt/c/`）。

3. **使用原生 Windows**：考虑在 Windows 上原生运行 Claude Code 而不是通过 WSL，以获得更好的文件系统性能。

## IDE 集成问题

### WSL2 上未检测到 JetBrains IDE

如果您在 WSL2 上使用 Claude Code 和 JetBrains IDE，并收到"未检测到可用的 IDE"错误，这可能是由于 WSL2 的网络配置或 Windows 防火墙阻止连接。

#### WSL2 网络模式

WSL2 默认使用 NAT 网络，这可能会阻止 IDE 检测。您有两个选项：

**选项 1：配置 Windows 防火墙**（推荐）

1. 查找您的 WSL2 IP 地址：
   ```bash  theme={null}
   wsl hostname -I
   # Example output: 172.21.123.456
   ```

2. 以管理员身份打开 PowerShell 并创建防火墙规则：
   ```powershell  theme={null}
   New-NetFirewallRule -DisplayName "Allow WSL2 Internal Traffic" -Direction Inbound -Protocol TCP -Action Allow -RemoteAddress 172.21.0.0/16 -LocalAddress 172.21.0.0/16
   ```
   （根据第 1 步中的 WSL2 子网调整 IP 范围）

3. 重新启动您的 IDE 和 Claude Code

**选项 2：切换到镜像网络**

添加到 Windows 用户目录中的 `.wslconfig`：

```ini  theme={null}
[wsl2]
networkingMode=mirrored
```

然后从 PowerShell 使用 `wsl --shutdown` 重新启动 WSL。

<Note>
  这些网络问题仅影响 WSL2。WSL1 直接使用主机的网络，不需要这些配置。
</Note>

有关其他 JetBrains 配置提示，请参阅我们的 [JetBrains IDE 指南](/zh-CN/jetbrains#plugin-settings)。

### 报告 Windows IDE 集成问题（原生和 WSL）

如果您在 Windows 上遇到 IDE 集成问题，请[创建一个问题](https://github.com/anthropics/claude-code/issues)并提供以下信息：

* 环境类型：原生 Windows（Git Bash）或 WSL1/WSL2
* WSL 网络模式（如果适用）：NAT 或镜像
* IDE 名称和版本
* Claude Code 扩展/插件版本
* Shell 类型：Bash、Zsh、PowerShell 等

### JetBrains（IntelliJ、PyCharm 等）终端中的 Escape 键不起作用

如果您在 JetBrains 终端中使用 Claude Code，而 `Esc` 键不能按预期中断代理，这可能是由于 JetBrains 默认快捷键的键绑定冲突。

要解决此问题：

1. 转到设置 → 工具 → 终端
2. 要么：
   * 取消选中"使用 Escape 将焦点移到编辑器"，或
   * 单击"配置终端键绑定"并删除"切换焦点到编辑器"快捷键
3. 应用更改

这允许 `Esc` 键正确中断 Claude Code 操作。

## Markdown 格式问题

Claude Code 有时会生成 Markdown 文件，代码围栏上缺少语言标签，这可能会影响 GitHub、编辑器和文档工具中的语法突出显示和可读性。

### 代码块中缺少语言标签

如果您在生成的 Markdown 中注意到这样的代码块：

````markdown  theme={null}
```
function example() {
  return "hello";
}
```
````

而不是正确标记的块，如：

````markdown  theme={null}
```javascript
function example() {
  return "hello";
}
```
````

**解决方案：**

1. **要求 Claude 添加语言标签**：请求"为此 Markdown 文件中的所有代码块添加适当的语言标签。"

2. **使用后处理钩子**：设置自动格式化钩子来检测和添加缺少的语言标签。有关实现详细信息，请参阅 [Markdown 格式化钩子示例](/zh-CN/hooks-guide#markdown-formatting-hook)。

3. **手动验证**：生成 Markdown 文件后，查看它们以确保正确的代码块格式，如果需要，请求更正。

### 不一致的间距和格式

如果生成的 Markdown 有过多的空行或不一致的间距：

**解决方案：**

1. **请求格式更正**：要求 Claude"修复此 Markdown 文件中的间距和格式问题。"

2. **使用格式化工具**：设置钩子以在生成的 Markdown 文件上运行 Markdown 格式化程序（如 `prettier`）或自定义格式化脚本。

3. **指定格式首选项**：在您的提示或项目[内存](/zh-CN/memory)文件中包含格式要求。

### Markdown 生成的最佳实践

要最小化格式问题：

* **在请求中明确说明**：要求"格式正确的 Markdown，带有语言标记的代码块"
* **使用项目约定**：在 [`CLAUDE.md`](/zh-CN/memory) 中记录您首选的 Markdown 风格
* **设置验证钩子**：使用后处理钩子自动验证和修复常见格式问题

## 获取更多帮助

如果您遇到此处未涵盖的问题：

1. 在 Claude Code 中使用 `/bug` 命令直接向 Anthropic 报告问题
2. 检查 [GitHub 存储库](https://github.com/anthropics/claude-code)以了解已知问题
3. 运行 `/doctor` 检查您的 Claude Code 安装的健康状况
4. 直接询问 Claude 其功能和特性 - Claude 内置了对其文档的访问权限
