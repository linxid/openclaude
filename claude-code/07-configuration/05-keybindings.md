---
title: "自定义快捷键"
description: "使用快捷键配置文件在 Claude Code 中自定义快捷键。"
---

# 自定义快捷键

> 使用快捷键配置文件在 Claude Code 中自定义快捷键。

<Warning>此功能目前处于预览阶段，并非所有用户都可使用。</Warning>

Claude Code 支持可自定义的快捷键。运行 `/keybindings` 来创建或打开位于 `~/.claude/keybindings.json` 的配置文件。

## 配置文件

快捷键配置文件是一个包含 `bindings` 数组的对象。每个块指定一个上下文和一个按键映射到操作的映射。

<Note>快捷键文件的更改会自动检测并应用，无需重启 Claude Code。</Note>

| 字段         | 描述                            |
| :--------- | :---------------------------- |
| `$schema`  | 可选的 JSON Schema URL，用于编辑器自动完成 |
| `$docs`    | 可选的文档 URL                     |
| `bindings` | 按上下文分组的绑定块数组                  |

此示例将 `Ctrl+E` 绑定到在聊天上下文中打开外部编辑器，并取消绑定 `Ctrl+U`：

```json  theme={null}
{
  "$schema": "https://code.claude.com/docs/schemas/keybindings.json",
  "$docs": "https://code.claude.com/docs/s/claude-code-keybindings",
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+e": "chat:externalEditor",
        "ctrl+u": null
      }
    }
  ]
}
```

## 上下文

每个绑定块指定一个**上下文**，其中绑定适用：

| 上下文               | 描述                 |
| :---------------- | :----------------- |
| `Global`          | 在应用程序中的任何地方应用      |
| `Chat`            | 主聊天输入区域            |
| `Autocomplete`    | 自动完成菜单已打开          |
| `Settings`        | 设置菜单（仅限 Escape 关闭） |
| `Confirmation`    | 权限和确认对话框           |
| `Tabs`            | 选项卡导航组件            |
| `Help`            | 帮助菜单可见             |
| `Transcript`      | 记录查看器              |
| `HistorySearch`   | 历史搜索模式（Ctrl+R）     |
| `Task`            | 后台任务正在运行           |
| `ThemePicker`     | 主题选择器对话框           |
| `Attachments`     | 图像/附件栏导航           |
| `Footer`          | 页脚指示器导航（任务、团队、差异）  |
| `MessageSelector` | 回退对话框消息选择          |
| `DiffDialog`      | 差异查看器导航            |
| `ModelPicker`     | 模型选择器工作量级别         |
| `Select`          | 通用选择/列表组件          |
| `Plugin`          | 插件对话框（浏览、发现、管理）    |

## 可用操作

操作遵循 `namespace:action` 格式，例如 `chat:submit` 发送消息或 `app:toggleTodos` 显示任务列表。每个上下文都有特定的可用操作。

### 应用程序操作

在 `Global` 上下文中可用的操作：

| 操作                     | 默认     | 描述             |
| :--------------------- | :----- | :------------- |
| `app:interrupt`        | Ctrl+C | 取消当前操作         |
| `app:exit`             | Ctrl+D | 退出 Claude Code |
| `app:toggleTodos`      | Ctrl+T | 切换任务列表可见性      |
| `app:toggleTranscript` | Ctrl+O | 切换详细记录         |

### 历史操作

用于导航命令历史的操作：

| 操作                 | 默认     | 描述     |
| :----------------- | :----- | :----- |
| `history:search`   | Ctrl+R | 打开历史搜索 |
| `history:previous` | Up     | 上一个历史项 |
| `history:next`     | Down   | 下一个历史项 |

### 聊天操作

在 `Chat` 上下文中可用的操作：

| 操作                    | 默认                       | 描述        |
| :-------------------- | :----------------------- | :-------- |
| `chat:cancel`         | Escape                   | 取消当前输入    |
| `chat:cycleMode`      | Shift+Tab\*              | 循环权限模式    |
| `chat:modelPicker`    | Cmd+P / Meta+P           | 打开模型选择器   |
| `chat:thinkingToggle` | Cmd+T / Meta+T           | 切换扩展思考    |
| `chat:submit`         | Enter                    | 提交消息      |
| `chat:undo`           | Ctrl+\_                  | 撤销上一个操作   |
| `chat:externalEditor` | Ctrl+G                   | 在外部编辑器中打开 |
| `chat:stash`          | Ctrl+S                   | 隐藏当前提示    |
| `chat:imagePaste`     | Ctrl+V（Windows 上为 Alt+V） | 粘贴图像      |

\*在没有 VT 模式的 Windows 上（Node \<24.2.0/\<22.17.0，Bun \<1.2.23），默认为 Meta+M。

### 自动完成操作

在 `Autocomplete` 上下文中可用的操作：

| 操作                      | 默认     | 描述    |
| :---------------------- | :----- | :---- |
| `autocomplete:accept`   | Tab    | 接受建议  |
| `autocomplete:dismiss`  | Escape | 关闭菜单  |
| `autocomplete:previous` | Up     | 上一个建议 |
| `autocomplete:next`     | Down   | 下一个建议 |

### 确认操作

在 `Confirmation` 上下文中可用的操作：

| 操作                          | 默认        | 描述     |
| :-------------------------- | :-------- | :----- |
| `confirm:yes`               | Y, Enter  | 确认操作   |
| `confirm:no`                | N, Escape | 拒绝操作   |
| `confirm:previous`          | Up        | 上一个选项  |
| `confirm:next`              | Down      | 下一个选项  |
| `confirm:nextField`         | Tab       | 下一个字段  |
| `confirm:previousField`     | （未绑定）     | 上一个字段  |
| `confirm:cycleMode`         | Shift+Tab | 循环权限模式 |
| `confirm:toggleExplanation` | Ctrl+E    | 切换权限说明 |

### 权限操作

在 `Confirmation` 上下文中可用于权限对话框的操作：

| 操作                       | 默认     | 描述       |
| :----------------------- | :----- | :------- |
| `permission:toggleDebug` | Ctrl+D | 切换权限调试信息 |

### 记录操作

在 `Transcript` 上下文中可用的操作：

| 操作                         | 默认             | 描述       |
| :------------------------- | :------------- | :------- |
| `transcript:toggleShowAll` | Ctrl+E         | 切换显示所有内容 |
| `transcript:exit`          | Ctrl+C, Escape | 退出记录查看   |

### 历史搜索操作

在 `HistorySearch` 上下文中可用的操作：

| 操作                      | 默认          | 描述      |
| :---------------------- | :---------- | :------ |
| `historySearch:next`    | Ctrl+R      | 下一个匹配项  |
| `historySearch:accept`  | Escape, Tab | 接受选择    |
| `historySearch:cancel`  | Ctrl+C      | 取消搜索    |
| `historySearch:execute` | Enter       | 执行选定的命令 |

### 任务操作

在 `Task` 上下文中可用的操作：

| 操作                | 默认     | 描述     |
| :---------------- | :----- | :----- |
| `task:background` | Ctrl+B | 后台当前任务 |

### 主题操作

在 `ThemePicker` 上下文中可用的操作：

| 操作                               | 默认     | 描述     |
| :------------------------------- | :----- | :----- |
| `theme:toggleSyntaxHighlighting` | Ctrl+T | 切换语法高亮 |

### 帮助操作

在 `Help` 上下文中可用的操作：

| 操作             | 默认     | 描述     |
| :------------- | :----- | :----- |
| `help:dismiss` | Escape | 关闭帮助菜单 |

### 选项卡操作

在 `Tabs` 上下文中可用的操作：

| 操作              | 默认              | 描述     |
| :-------------- | :-------------- | :----- |
| `tabs:next`     | Tab, Right      | 下一个选项卡 |
| `tabs:previous` | Shift+Tab, Left | 上一个选项卡 |

### 附件操作

在 `Attachments` 上下文中可用的操作：

| 操作                     | 默认                | 描述      |
| :--------------------- | :---------------- | :------ |
| `attachments:next`     | Right             | 下一个附件   |
| `attachments:previous` | Left              | 上一个附件   |
| `attachments:remove`   | Backspace, Delete | 删除选定的附件 |
| `attachments:exit`     | Down, Escape      | 退出附件栏   |

### 页脚操作

在 `Footer` 上下文中可用的操作：

| 操作                      | 默认     | 描述       |
| :---------------------- | :----- | :------- |
| `footer:next`           | Right  | 下一个页脚项   |
| `footer:previous`       | Left   | 上一个页脚项   |
| `footer:openSelected`   | Enter  | 打开选定的页脚项 |
| `footer:clearSelection` | Escape | 清除页脚选择   |

### 消息选择器操作

在 `MessageSelector` 上下文中可用的操作：

| 操作                       | 默认                                        | 描述       |
| :----------------------- | :---------------------------------------- | :------- |
| `messageSelector:up`     | Up, K                                     | 在列表中向上移动 |
| `messageSelector:down`   | Down, J                                   | 在列表中向下移动 |
| `messageSelector:top`    | Ctrl+Up, Shift+Up, Meta+Up, Shift+K       | 跳到顶部     |
| `messageSelector:bottom` | Ctrl+Down, Shift+Down, Meta+Down, Shift+J | 跳到底部     |
| `messageSelector:select` | Enter                                     | 选择消息     |

### 差异操作

在 `DiffDialog` 上下文中可用的操作：

| 操作                    | 默认       | 描述        |
| :-------------------- | :------- | :-------- |
| `diff:dismiss`        | Escape   | 关闭差异查看器   |
| `diff:previousSource` | Left     | 上一个差异源    |
| `diff:nextSource`     | Right    | 下一个差异源    |
| `diff:previousFile`   | Up       | 差异中的上一个文件 |
| `diff:nextFile`       | Down     | 差异中的下一个文件 |
| `diff:viewDetails`    | Enter    | 查看差异详情    |
| `diff:back`           | （特定于上下文） | 在差异查看器中返回 |

### 模型选择器操作

在 `ModelPicker` 上下文中可用的操作：

| 操作                           | 默认    | 描述      |
| :--------------------------- | :---- | :------ |
| `modelPicker:decreaseEffort` | Left  | 降低工作量级别 |
| `modelPicker:increaseEffort` | Right | 提高工作量级别 |

### 选择操作

在 `Select` 上下文中可用的操作：

| 操作                | 默认              | 描述    |
| :---------------- | :-------------- | :---- |
| `select:next`     | Down, J, Ctrl+N | 下一个选项 |
| `select:previous` | Up, K, Ctrl+P   | 上一个选项 |
| `select:accept`   | Enter           | 接受选择  |
| `select:cancel`   | Escape          | 取消选择  |

### 插件操作

在 `Plugin` 上下文中可用的操作：

| 操作               | 默认    | 描述      |
| :--------------- | :---- | :------ |
| `plugin:toggle`  | Space | 切换插件选择  |
| `plugin:install` | I     | 安装选定的插件 |

### 设置操作

在 `Settings` 上下文中可用的操作：

| 操作                | 默认 | 描述            |
| :---------------- | :- | :------------ |
| `settings:search` | /  | 进入搜索模式        |
| `settings:retry`  | R  | 重试加载使用数据（出错时） |

## 按键语法

### 修饰符

使用修饰符键与 `+` 分隔符：

* `ctrl` 或 `control` - Control 键
* `alt`、`opt` 或 `option` - Alt/Option 键
* `shift` - Shift 键
* `meta`、`cmd` 或 `command` - Meta/Command 键

例如：

```
ctrl+k          单个键与修饰符
shift+tab       Shift + Tab
meta+p          Command/Meta + P
ctrl+shift+c    多个修饰符
```

### 大写字母

独立的大写字母意味着 Shift。例如，`K` 等同于 `shift+k`。这对于 vim 风格的绑定很有用，其中大写和小写键有不同的含义。

带有修饰符的大写字母（例如 `ctrl+K`）被视为风格性的，**不**意味着 Shift — `ctrl+K` 与 `ctrl+k` 相同。

### 和弦

和弦是由空格分隔的按键序列：

```
ctrl+k ctrl+s   按 Ctrl+K，释放，然后按 Ctrl+S
```

### 特殊键

* `escape` 或 `esc` - Escape 键
* `enter` 或 `return` - Enter 键
* `tab` - Tab 键
* `space` - 空格键
* `up`、`down`、`left`、`right` - 箭头键
* `backspace`、`delete` - 删除键

## 取消绑定默认快捷键

将操作设置为 `null` 以取消绑定默认快捷键：

```json  theme={null}
{
  "bindings": [
    {
      "context": "Chat",
      "bindings": {
        "ctrl+s": null
      }
    }
  ]
}
```

## 保留快捷键

这些快捷键无法重新绑定：

| 快捷键    | 原因        |
| :----- | :-------- |
| Ctrl+C | 硬编码的中断/取消 |
| Ctrl+D | 硬编码的退出    |

## 终端冲突

某些快捷键可能与终端多路复用器冲突：

| 快捷键    | 冲突                 |
| :----- | :----------------- |
| Ctrl+B | tmux 前缀（按两次发送）     |
| Ctrl+A | GNU screen 前缀      |
| Ctrl+Z | Unix 进程暂停（SIGTSTP） |

## Vim 模式交互

启用 vim 模式（`/vim`）时，快捷键和 vim 模式独立运行：

* **Vim 模式**在文本输入级别处理输入（光标移动、模式、动作）
* **快捷键**在组件级别处理操作（切换待办事项、提交等）
* vim 模式中的 Escape 键从 INSERT 切换到 NORMAL 模式；它不触发 `chat:cancel`
* 大多数 Ctrl+key 快捷键通过 vim 模式传递到快捷键系统
* 在 vim NORMAL 模式中，`?` 显示帮助菜单（vim 行为）

## 验证

Claude Code 验证您的快捷键并显示以下警告：

* 解析错误（无效的 JSON 或结构）
* 无效的上下文名称
* 保留快捷键冲突
* 终端多路复用器冲突
* 同一上下文中的重复绑定

运行 `/doctor` 查看任何快捷键警告。
