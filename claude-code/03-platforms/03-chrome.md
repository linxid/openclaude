---
title: "在 Chrome 中使用 Claude Code（测试版）"
description: "将 Claude Code 连接到 Chrome 浏览器，以测试网络应用、使用控制台日志进行调试、自动填充表单以及从网页中提取数据。"
---

Claude Code 与 Claude in Chrome 浏览器扩展程序集成，为您提供从 CLI 或 [VS Code 扩展程序](/zh-CN/vs-code#automate-browser-tasks-with-chrome) 进行浏览器自动化的功能。构建您的代码，然后在浏览器中测试和调试，无需切换上下文。

Claude 为浏览器任务打开新标签页，并共享您浏览器的登录状态，因此它可以访问您已登录的任何网站。浏览器操作在实时可见的 Chrome 窗口中运行。当 Claude 遇到登录页面或 CAPTCHA 时，它会暂停并要求您手动处理。

<Note>
  Chrome 集成处于测试版阶段，目前仅适用于 Google Chrome。尚不支持 Brave、Arc 或其他基于 Chromium 的浏览器。也不支持 WSL（Windows 子系统 for Linux）。
</Note>

## 功能

连接 Chrome 后，您可以在单个工作流中链接浏览器操作和编码任务：

* **实时调试**：直接读取控制台错误和 DOM 状态，然后修复导致这些错误的代码
* **设计验证**：从 Figma 模型构建 UI，然后在浏览器中打开它以验证它是否匹配
* **网络应用测试**：测试表单验证、检查视觉回归或验证用户流程
* **已认证的网络应用**：与 Google Docs、Gmail、Notion 或您已登录的任何应用交互，无需 API 连接器
* **数据提取**：从网页中提取结构化信息并将其保存到本地
* **任务自动化**：自动化重复的浏览器任务，如数据输入、表单填充或多站点工作流
* **会话录制**：将浏览器交互录制为 GIF，以记录或分享发生的情况

## 前置条件

在使用 Claude Code 与 Chrome 之前，您需要：

* [Google Chrome](https://www.google.com/chrome/) 浏览器
* [Claude in Chrome 扩展程序](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn) 版本 1.0.36 或更高版本
* [Claude Code](/zh-CN/quickstart#step-1-install-claude-code) 版本 2.0.73 或更高版本
* 直接 Anthropic 计划（Pro、Max、Team 或 Enterprise）

<Note>
  Chrome 集成不可通过 Amazon Bedrock、Google Cloud Vertex AI 或 Microsoft Foundry 等第三方提供商获得。如果您仅通过第三方提供商访问 Claude，则需要单独的 claude.ai 账户来使用此功能。
</Note>

## 在 CLI 中开始

<Steps>
  <Step title="使用 Chrome 启动 Claude Code">
    使用 `--chrome` 标志启动 Claude Code：

    ```bash  theme={null}
    claude --chrome
    ```

    您也可以通过在现有会话中运行 `/chrome` 来从现有会话中启用 Chrome。
  </Step>

  <Step title="要求 Claude 使用浏览器">
    此示例导航到页面、与其交互并报告其发现，全部来自您的终端或编辑器：

    ```text  theme={null}
    Go to code.claude.com/docs, click on the search box,
    type "hooks", and tell me what results appear
    ```
  </Step>
</Steps>

随时运行 `/chrome` 以检查连接状态、管理权限或重新连接扩展程序。

对于 VS Code，请参阅 [VS Code 中的浏览器自动化](/zh-CN/vs-code#automate-browser-tasks-with-chrome)。

### 默认启用 Chrome

为了避免每个会话都传递 `--chrome`，运行 `/chrome` 并选择"默认启用"。

在 [VS Code 扩展程序](/zh-CN/vs-code#automate-browser-tasks-with-chrome) 中，只要安装了 Chrome 扩展程序，Chrome 就可用。无需额外标志。

<Note>
  在 CLI 中默认启用 Chrome 会增加上下文使用，因为浏览器工具始终被加载。如果您注意到上下文消耗增加，请禁用此设置，仅在需要时使用 `--chrome`。
</Note>

### 管理网站权限

网站级权限从 Chrome 扩展程序继承。在 Chrome 扩展程序设置中管理权限，以控制 Claude 可以浏览、点击和输入的网站。

## 示例工作流

这些示例展示了将浏览器操作与编码任务结合的常见方式。运行 `/mcp` 并选择 `claude-in-chrome` 以查看可用浏览器工具的完整列表。

### 测试本地网络应用

在开发网络应用时，要求 Claude 验证您的更改是否正常工作：

```text  theme={null}
I just updated the login form validation. Can you open localhost:3000,
try submitting the form with invalid data, and check if the error
messages appear correctly?
```

Claude 导航到您的本地服务器、与表单交互并报告其观察到的内容。

### 使用控制台日志进行调试

Claude 可以读取控制台输出以帮助诊断问题。告诉 Claude 要查找的模式，而不是要求所有控制台输出，因为日志可能很冗长：

```text  theme={null}
Open the dashboard page and check the console for any errors when
the page loads.
```

Claude 读取控制台消息，可以过滤特定模式或错误类型。

### 自动填充表单

加快重复数据输入任务的速度：

```text  theme={null}
I have a spreadsheet of customer contacts in contacts.csv. For each row,
go to the CRM at crm.example.com, click "Add Contact", and fill in the
name, email, and phone fields.
```

Claude 读取您的本地文件、导航网络界面并为每条记录输入数据。

### 在 Google Docs 中起草内容

使用 Claude 直接在您的文档中写入，无需 API 设置：

```text  theme={null}
Draft a project update based on the recent commits and add it to my
Google Doc at docs.google.com/document/d/abc123
```

Claude 打开文档、点击编辑器并输入内容。这适用于您已登录的任何网络应用：Gmail、Notion、Sheets 等。

### 从网页中提取数据

从网站中提取结构化信息：

```text  theme={null}
Go to the product listings page and extract the name, price, and
availability for each item. Save the results as a CSV file.
```

Claude 导航到页面、读取内容并将数据编译成结构化格式。

### 运行多站点工作流

协调多个网站之间的任务：

```text  theme={null}
Check my calendar for meetings tomorrow, then for each meeting with
an external attendee, look up their company website and add a note
about what they do.
```

Claude 跨标签页工作以收集信息并完成工作流。

### 录制演示 GIF

创建浏览器交互的可共享录制：

```text  theme={null}
Record a GIF showing how to complete the checkout flow, from adding
an item to the cart through to the confirmation page.
```

Claude 录制交互序列并将其保存为 GIF 文件。

## 故障排除

### 未检测到扩展程序

如果 Claude Code 显示"未检测到 Chrome 扩展程序"：

1. 验证 Chrome 扩展程序已安装并在 `chrome://extensions` 中启用
2. 通过运行 `claude --version` 验证 Claude Code 是最新的
3. 检查 Chrome 是否正在运行
4. 运行 `/chrome` 并选择"重新连接扩展程序"以重新建立连接
5. 如果问题仍然存在，请重新启动 Claude Code 和 Chrome

第一次启用 Chrome 集成时，Claude Code 会安装本机消息传递主机配置文件。Chrome 在启动时读取此文件，因此如果扩展程序在您的第一次尝试中未被检测到，请重新启动 Chrome 以获取新配置。

如果连接仍然失败，请验证主机配置文件是否存在于：

* **macOS**：`~/Library/Application Support/Google/Chrome/NativeMessagingHosts/com.anthropic.claude_code_browser_extension.json`
* **Linux**：`~/.config/google-chrome/NativeMessagingHosts/com.anthropic.claude_code_browser_extension.json`
* **Windows**：检查 Windows 注册表中的 `HKCU\Software\Google\Chrome\NativeMessagingHosts\`

### 浏览器无响应

如果 Claude 的浏览器命令停止工作：

1. 检查是否有模态对话框（alert、confirm、prompt）阻止页面。JavaScript 对话框阻止浏览器事件并防止 Claude 接收命令。手动关闭对话框，然后告诉 Claude 继续。
2. 要求 Claude 创建新标签页并重试
3. 通过在 `chrome://extensions` 中禁用并重新启用来重新启动 Chrome 扩展程序

### 长会话期间连接断开

Chrome 扩展程序的 service worker 在扩展会话期间可能会进入空闲状态，这会破坏连接。如果浏览器工具在一段时间不活动后停止工作，请运行 `/chrome` 并选择"重新连接扩展程序"。

### Windows 特定问题

在 Windows 上，您可能会遇到：

* **命名管道冲突 (EADDRINUSE)**：如果另一个进程正在使用相同的命名管道，请重新启动 Claude Code。关闭任何可能使用 Chrome 的其他 Claude Code 会话。
* **本机消息传递主机错误**：如果本机消息传递主机在启动时崩溃，请尝试重新安装 Claude Code 以重新生成主机配置。

### 常见错误消息

这些是最常见的错误及其解决方法：

| 错误           | 原因                         | 修复                                             |
| ------------ | -------------------------- | ---------------------------------------------- |
| "浏览器扩展程序未连接" | 本机消息传递主机无法到达扩展程序           | 重新启动 Chrome 和 Claude Code，然后运行 `/chrome` 以重新连接 |
| "未检测到扩展程序"   | Chrome 扩展程序未安装或已禁用         | 在 `chrome://extensions` 中安装或启用扩展程序             |
| "没有可用的标签页"   | Claude 在标签页准备好之前尝试操作       | 要求 Claude 创建新标签页并重试                            |
| "接收端不存在"     | 扩展程序 service worker 进入空闲状态 | 运行 `/chrome` 并选择"重新连接扩展程序"                     |

## 另请参阅

* [在 VS Code 中使用 Claude Code](/zh-CN/vs-code#automate-browser-tasks-with-chrome)：VS Code 扩展程序中的浏览器自动化
* [CLI 参考](/zh-CN/cli-reference)：命令行标志，包括 `--chrome`
* [常见工作流](/zh-CN/common-workflows)：更多使用 Claude Code 的方式
* [数据和隐私](/zh-CN/data-usage)：Claude Code 如何处理您的数据
* [Claude in Chrome 入门](https://support.claude.com/en/articles/12012173-getting-started-with-claude-in-chrome)：Chrome 扩展程序的完整文档，包括快捷键、计划和权限
