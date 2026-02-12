---
title: "Claude Code 桌面版"
description: "在本地或安全的云基础设施上运行 Claude Code 任务，使用 Claude 桌面应用"
---

<img src="https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=c4e9dc9737b437d36ab253b75a1cc595" alt="Claude Code 桌面版界面" data-og-width="4132" width="4132" data-og-height="2620" height="2620" data-path="images/desktop-interface.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=280&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=b1a8421a544c3e8c78679fa1a7b56190 280w, https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=560&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=79cf4ea0923098cc429198678ea50903 560w, https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=840&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=14bcbcd569d179770fe656686ffbf6bf 840w, https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=1100&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=b873274db1e9ff8585ba545032aa24a5 1100w, https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=1650&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=25553dced783c3a8c2a1134a53295f7e 1650w, https://mintcdn.com/claude-code/zEGbGSbinVtT3BLw/images/desktop-interface.png?w=2500&fit=max&auto=format&n=zEGbGSbinVtT3BLw&q=85&s=9ad49e6468c2f87b1895093deeea7bb2 2500w" />

## Claude Code 桌面版（预览版）

Claude 桌面应用为在本地机器上运行多个 Claude Code 会话提供了原生界面，并与网页版 Claude Code 无缝集成。

## 安装

为您的平台下载 Claude 桌面应用：

<CardGroup cols={2}>
  <Card title="macOS" icon="apple" href="https://claude.ai/api/desktop/darwin/universal/dmg/latest/redirect?utm_source=claude_code&utm_medium=docs">
    适用于 Intel 和 Apple Silicon 的通用版本
  </Card>

  <Card title="Windows" icon="windows" href="https://claude.ai/api/desktop/win32/x64/exe/latest/redirect?utm_source=claude_code&utm_medium=docs">
    适用于 x64 处理器
  </Card>
</CardGroup>

对于 Windows ARM64，[在此下载](https://claude.ai/api/desktop/win32/arm64/exe/latest/redirect?utm_source=claude_code\&utm_medium=docs)。

<Note>
  Windows ARM64 上不提供本地会话。
</Note>

## 功能

Claude Code 桌面版提供：

* **使用 `git` worktrees 的并行本地会话**：在同一存储库中同时运行多个 Claude Code 会话，每个会话都有自己独立的 `git` worktree
* **在 worktrees 中包含 `.gitignore` 中列出的文件**：使用 `.worktreeinclude` 自动将 `.gitignore` 中的文件（如 `.env`）复制到新的 worktrees
* **启动网页版 Claude Code**：直接从桌面应用启动在 Anthropic 安全云基础设施上运行的会话

## 使用 Git worktrees

Claude Code 桌面版支持在同一存储库中使用 Git worktrees 运行多个 Claude Code 会话。每个会话都有自己的独立 worktree，允许 Claude 在不同任务上工作而不会产生冲突。worktrees 的默认位置是 `~/.claude-worktrees`，但可以在 Claude 桌面应用的设置中配置。

<Note>
  如果您在未初始化 Git 的文件夹中启动本地会话，桌面应用将不会创建新的 worktree。
</Note>

### 复制 `.gitignore` 中忽略的文件

当 Claude Code 创建 worktree 时，通过 `.gitignore` 忽略的文件不会自动可用。包含 `.worktreeinclude` 文件可以解决这个问题，它指定哪些被忽略的文件应该复制到新的 worktrees。

在存储库根目录中创建 `.worktreeinclude` 文件：

```
.env
.env.local
.env.*
**/.claude/settings.local.json
```

该文件使用 `.gitignore` 风格的模式。创建 worktree 时，与这些模式匹配且也在 `.gitignore` 中的文件将从主存储库复制到 worktree。

<Tip>
  只有同时与 `.worktreeinclude` 匹配且列在 `.gitignore` 中的文件才会被复制。这可以防止意外复制跟踪的文件。
</Tip>

### 启动网页版 Claude Code

从桌面应用，您可以启动在 Anthropic 安全云基础设施上运行的 Claude Code 会话。

要从桌面启动网页会话，在创建新会话时选择远程环境。

有关更多详情，请参阅 [网页版 Claude Code](/claude-code/03-platforms/01-claude-code-on-the-web)。

## 捆绑的 Claude Code 版本

Claude Code 桌面版包含一个捆绑的稳定版本的 Claude Code，以确保所有桌面用户获得一致的体验。即使计算机上已存在 Claude Code 版本，捆绑版本也是必需的，并在首次启动时下载。桌面应用自动管理版本更新并清理旧版本。

<Note>
  桌面版中捆绑的 Claude Code 版本可能与最新的 CLI 版本不同。桌面版优先考虑稳定性，而 CLI 可能具有较新的功能。
</Note>

## 环境配置

对于本地环境，Claude Code 桌面版自动从您的 shell 配置中提取 `$PATH` 环境变量。这允许本地会话访问开发工具，如 `yarn`、`npm`、`node` 和终端中可用的其他命令，无需额外设置。

### 自定义环境变量

选择"本地"环境，然后在右侧选择设置按钮。这将打开一个对话框，您可以在其中更新本地环境变量。这对于设置项目特定的变量或开发工作流所需的 API 密钥很有用。出于安全原因，环境变量值在 UI 中被掩盖。

<Note>
  环境变量必须指定为键值对，采用 [`.env` 格式](https://www.dotenv.org/)。例如：

  ```
  API_KEY=your_api_key
  DEBUG=true

  # 多行值 - 用引号包装
  CERT="---BEGIN CERT---
  MIIE...
  ---END CERT---"
  ```
</Note>

## 企业配置

组织可以使用 `isClaudeCodeForDesktopEnabled` [企业策略选项](https://support.claude.com/en/articles/12622667-enterprise-configuration#h_003283c7cb) 在桌面应用中禁用本地 Claude Code 使用。此外，可以在您的 [管理员设置](https://claude.ai/admin-settings/claude-code) 中禁用网页版 Claude Code。

## 相关资源

* [网页版 Claude Code](/claude-code/03-platforms/01-claude-code-on-the-web)
* [Claude 桌面支持文章](https://support.claude.com/en/collections/16163169-claude-desktop)
* [企业配置](https://support.claude.com/en/articles/12622667-enterprise-configuration)
* [常见工作流](/claude-code/02-core-concepts/03-common-workflows)
* [设置参考](/claude-code/07-configuration/01-settings)
