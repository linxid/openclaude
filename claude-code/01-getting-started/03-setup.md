---
title: "设置 Claude Code"
description: "在您的开发机器上安装、认证和开始使用 Claude Code。"
---

# 设置 Claude Code

> 在您的开发机器上安装、认证和开始使用 Claude Code。

## 系统要求

* **操作系统**：macOS 13.0+、Ubuntu 20.04+/Debian 10+ 或 Windows 10+（带 WSL 1、WSL 2 或 Git for Windows）
* **硬件**：4 GB+ RAM
* **网络**：需要互联网连接（请参阅[网络配置](/zh-CN/network-config#network-access-requirements)）
* **Shell**：在 Bash 或 Zsh 中效果最佳
* **位置**：[Anthropic 支持的国家/地区](https://www.anthropic.com/supported-countries)

### 其他依赖项

* **ripgrep**：通常包含在 Claude Code 中。如果搜索失败，请参阅[搜索故障排除](/zh-CN/troubleshooting#search-and-discovery-issues)。
* **[Node.js 18+](https://nodejs.org/en/download)**：仅对[已弃用的 npm 安装](#npm-installation-deprecated)需要

## 安装

To install Claude Code, use one of the following methods:

<Tabs>
  <Tab title="Native Install (Recommended)">
    **macOS, Linux, WSL:**

    ```bash  theme={null}
    curl -fsSL https://claude.ai/install.sh | bash
    ```

    **Windows PowerShell:**

    ```powershell  theme={null}
    irm https://claude.ai/install.ps1 | iex
    ```

    **Windows CMD:**

    ```batch  theme={null}
    curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
    ```

    <Info>
      Native installations automatically update in the background to keep you on the latest version.
    </Info>
  </Tab>

  <Tab title="Homebrew">
    ```sh  theme={null}
    brew install --cask claude-code
    ```

    <Info>
      Homebrew installations do not auto-update. Run `brew upgrade claude-code` periodically to get the latest features and security fixes.
    </Info>
  </Tab>

  <Tab title="WinGet">
    ```powershell  theme={null}
    winget install Anthropic.ClaudeCode
    ```

    <Info>
      WinGet installations do not auto-update. Run `winget upgrade Anthropic.ClaudeCode` periodically to get the latest features and security fixes.
    </Info>
  </Tab>
</Tabs>

安装过程完成后，导航到您的项目并启动 Claude Code：

```bash  theme={null}
cd your-awesome-project
claude
```

如果您在安装过程中遇到任何问题，请查阅[故障排除指南](/zh-CN/troubleshooting)。

<Tip>
  安装后运行 `claude doctor` 以检查您的安装类型和版本。
</Tip>

<Note>
  **Alpine Linux 和其他基于 musl/uClibc 的发行版**：本机安装程序需要 `libgcc`、`libstdc++` 和 `ripgrep`。对于 Alpine：`apk add libgcc libstdc++ ripgrep`。设置 `USE_BUILTIN_RIPGREP=0`。
</Note>

### 认证

#### 对于个人用户

1. **Claude Pro 或 Max 计划**（推荐）：订阅 Claude 的 [Pro 或 Max 计划](https://claude.ai/pricing)，获得包括 Claude Code 和网页版 Claude 的统一订阅。在一个地方管理您的账户，并使用您的 Claude.ai 账户登录。
2. **Claude Console**：通过 [Claude Console](https://console.anthropic.com) 连接并完成 OAuth 流程。需要在 Anthropic Console 中有活跃的账单。系统会自动为使用情况跟踪和成本管理创建一个"Claude Code"工作区。您无法为 Claude Code 工作区创建 API 密钥；它专门用于 Claude Code 使用。

#### 对于团队和组织

1. **Claude for Teams 或 Enterprise**（推荐）：订阅 [Claude for Teams](https://claude.com/pricing#team-&-enterprise) 或 [Claude for Enterprise](https://anthropic.com/contact-sales)，以获得集中计费、团队管理以及对 Claude Code 和网页版 Claude 的访问权限。团队成员使用其 Claude.ai 账户登录。
2. **带有团队计费的 Claude Console**：设置共享的 [Claude Console](https://console.anthropic.com) 组织，并启用团队计费。邀请团队成员并分配角色以进行使用情况跟踪。
3. **云提供商**：配置 Claude Code 以使用 [Amazon Bedrock、Google Vertex AI 或 Microsoft Foundry](/zh-CN/third-party-integrations)，以便与您现有的云基础设施进行部署。

### 安装特定版本

本机安装程序接受特定版本号或发布渠道（`latest` 或 `stable`）。您在安装时选择的渠道将成为自动更新的默认渠道。有关更多信息，请参阅[配置发布渠道](#configure-release-channel)。

要安装最新版本（默认）：

<Tabs>
  <Tab title="macOS、Linux、WSL">
    ```bash  theme={null}
    curl -fsSL https://claude.ai/install.sh | bash
    ```
  </Tab>

  <Tab title="Windows PowerShell">
    ```powershell  theme={null}
    irm https://claude.ai/install.ps1 | iex
    ```
  </Tab>

  <Tab title="Windows CMD">
    ```batch  theme={null}
    curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
    ```
  </Tab>
</Tabs>

要安装稳定版本：

<Tabs>
  <Tab title="macOS、Linux、WSL">
    ```bash  theme={null}
    curl -fsSL https://claude.ai/install.sh | bash -s stable
    ```
  </Tab>

  <Tab title="Windows PowerShell">
    ```powershell  theme={null}
    & ([scriptblock]::Create((irm https://claude.ai/install.ps1))) stable
    ```
  </Tab>

  <Tab title="Windows CMD">
    ```batch  theme={null}
    curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd stable && del install.cmd
    ```
  </Tab>
</Tabs>

要安装特定版本号：

<Tabs>
  <Tab title="macOS、Linux、WSL">
    ```bash  theme={null}
    curl -fsSL https://claude.ai/install.sh | bash -s 1.0.58
    ```
  </Tab>

  <Tab title="Windows PowerShell">
    ```powershell  theme={null}
    & ([scriptblock]::Create((irm https://claude.ai/install.ps1))) 1.0.58
    ```
  </Tab>

  <Tab title="Windows CMD">
    ```batch  theme={null}
    curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd 1.0.58 && del install.cmd
    ```
  </Tab>
</Tabs>

### 二进制完整性和代码签名

* 所有平台的 SHA256 校验和发布在发布清单中，目前位于 `https://storage.googleapis.com/claude-code-dist-86c565f3-f756-42ad-8dfa-d59b1c096819/claude-code-releases/{VERSION}/manifest.json`（示例：将 `{VERSION}` 替换为 `2.0.30`）
* 签名的二进制文件分发用于以下平台：
  * macOS：由"Anthropic PBC"签名并由 Apple 公证
  * Windows：由"Anthropic, PBC"签名

## NPM 安装（已弃用）

NPM 安装已弃用。尽可能使用[本机安装](#installation)方法。要将现有 npm 安装迁移到本机安装，请运行 `claude install`。

**全局 npm 安装**

```sh  theme={null}
npm install -g @anthropic-ai/claude-code
```

<Warning>
  不要使用 `sudo npm install -g`，因为这可能导致权限问题和安全风险。
  如果您遇到权限错误，请参阅[故障排除权限错误](/zh-CN/troubleshooting#command-not-found-claude-or-permission-errors)以获取推荐的解决方案。
</Warning>

## Windows 设置

**选项 1：WSL 中的 Claude Code**

* 支持 WSL 1 和 WSL 2

**选项 2：使用 Git Bash 在本机 Windows 上运行 Claude Code**

* 需要 [Git for Windows](https://git-scm.com/downloads/win)
* 对于便携式 Git 安装，请指定 `bash.exe` 的路径：
  ```powershell  theme={null}
  $env:CLAUDE_CODE_GIT_BASH_PATH="C:\Program Files\Git\bin\bash.exe"
  ```

## 更新 Claude Code

### 自动更新

Claude Code 会自动保持最新状态，以确保您拥有最新的功能和安全修复。

* **更新检查**：在启动时和运行时定期执行
* **更新过程**：在后台自动下载和安装
* **通知**：安装更新时您会看到通知
* **应用更新**：更新在您下次启动 Claude Code 时生效

<Note>
  Homebrew 和 WinGet 安装不会自动更新。使用 `brew upgrade claude-code` 或 `winget upgrade Anthropic.ClaudeCode` 手动更新。

  **已知问题**：Claude Code 可能会在新版本在这些包管理器中可用之前通知您有更新。如果升级失败，请稍候后重试。
</Note>

### 配置发布渠道

使用 `autoUpdatesChannel` 设置配置 Claude Code 为自动更新和 `claude update` 遵循的发布渠道：

* `"latest"`（默认）：在新功能发布后立即接收
* `"stable"`：使用通常约一周前的版本，跳过有重大回归的发布

通过 `/config` → **自动更新渠道**配置此项，或将其添加到您的 [settings.json 文件](/zh-CN/settings)：

```json  theme={null}
{
  "autoUpdatesChannel": "stable"
}
```

对于企业部署，您可以使用[托管设置](/zh-CN/iam#managed-settings)在整个组织中强制执行一致的发布渠道。

### 禁用自动更新

在您的 shell 或 [settings.json 文件](/zh-CN/settings)中设置 `DISABLE_AUTOUPDATER` 环境变量：

```bash  theme={null}
export DISABLE_AUTOUPDATER=1
```

### 手动更新

```bash  theme={null}
claude update
```

## 卸载 Claude Code

如果您需要卸载 Claude Code，请按照您的安装方法的说明进行操作。

### 本机安装

删除 Claude Code 二进制文件和版本文件：

**macOS、Linux、WSL：**

```bash  theme={null}
rm -f ~/.local/bin/claude
rm -rf ~/.local/share/claude
```

**Windows PowerShell：**

```powershell  theme={null}
Remove-Item -Path "$env:USERPROFILE\.local\bin\claude.exe" -Force
Remove-Item -Path "$env:USERPROFILE\.local\share\claude" -Recurse -Force
```

**Windows CMD：**

```batch  theme={null}
del "%USERPROFILE%\.local\bin\claude.exe"
rmdir /s /q "%USERPROFILE%\.local\share\claude"
```

### Homebrew 安装

```bash  theme={null}
brew uninstall --cask claude-code
```

### WinGet 安装

```powershell  theme={null}
winget uninstall Anthropic.ClaudeCode
```

### NPM 安装

```bash  theme={null}
npm uninstall -g @anthropic-ai/claude-code
```

### 清理配置文件（可选）

<Warning>
  删除配置文件将删除您的所有设置、允许的工具、MCP 服务器配置和会话历史记录。
</Warning>

要删除 Claude Code 设置和缓存数据：

**macOS、Linux、WSL：**

```bash  theme={null}
# 删除用户设置和状态
rm -rf ~/.claude
rm ~/.claude.json

# 删除项目特定的设置（从您的项目目录运行）
rm -rf .claude
rm -f .mcp.json
```

**Windows PowerShell：**

```powershell  theme={null}
# 删除用户设置和状态
Remove-Item -Path "$env:USERPROFILE\.claude" -Recurse -Force
Remove-Item -Path "$env:USERPROFILE\.claude.json" -Force

# 删除项目特定的设置（从您的项目目录运行）
Remove-Item -Path ".claude" -Recurse -Force
Remove-Item -Path ".mcp.json" -Force
```

**Windows CMD：**

```batch  theme={null}
REM 删除用户设置和状态
rmdir /s /q "%USERPROFILE%\.claude"
del "%USERPROFILE%\.claude.json"

REM 删除项目特定的设置（从您的项目目录运行）
rmdir /s /q ".claude"
del ".mcp.json"
```
