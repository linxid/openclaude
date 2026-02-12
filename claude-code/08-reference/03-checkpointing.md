---
title: "checkpointing"
description: "自动跟踪和回退 Claude 的编辑，快速恢复不需要的更改。"
---

Claude Code 自动跟踪 Claude 在工作过程中的文件编辑，允许您快速撤销更改并回退到之前的状态，以防任何事情出现偏差。

## checkpointing 如何工作

当您与 Claude 合作时，checkpointing 会自动捕获每次编辑前代码的状态。这个安全网让您可以放心地执行雄心勃勃的大规模任务，因为您始终可以返回到之前的代码状态。

### 自动跟踪

Claude Code 跟踪其文件编辑工具所做的所有更改：

* 每个用户提示都会创建一个新的 checkpoint
* Checkpoints 在会话之间持久存在，因此您可以在恢复的对话中访问它们
* 在 30 天后自动清理（可配置）

### 回退更改

按两次 `Esc`（`Esc` + `Esc`）或使用 `/rewind` 命令打开回退菜单。您可以选择恢复：

* **仅对话**：回退到用户消息，同时保留代码更改
* **仅代码**：恢复文件更改，同时保留对话
* **代码和对话**：将两者都恢复到会话中的先前点

## 常见用例

Checkpoints 在以下情况下特别有用：

* **探索替代方案**：尝试不同的实现方法，而不会丢失起点
* **从错误中恢复**：快速撤销引入错误或破坏功能的更改
* **迭代功能**：进行变体实验，同时知道您可以恢复到工作状态

## 限制

### Bash 命令更改未被跟踪

Checkpointing 不跟踪由 bash 命令修改的文件。例如，如果 Claude Code 运行：

```bash  theme={null}
rm file.txt
mv old.txt new.txt
cp source.txt dest.txt
```

这些文件修改无法通过回退撤销。只有通过 Claude 的文件编辑工具进行的直接文件编辑才会被跟踪。

### 外部更改未被跟踪

Checkpointing 仅跟踪在当前会话中已编辑的文件。您在 Claude Code 外部对文件所做的手动更改以及来自其他并发会话的编辑通常不会被捕获，除非它们碰巧修改了与当前会话相同的文件。

### 不是版本控制的替代品

Checkpoints 设计用于快速的会话级恢复。对于永久版本历史和协作：

* 继续使用版本控制（例如 Git）进行提交、分支和长期历史
* Checkpoints 补充但不替代适当的版本控制
* 将 checkpoints 视为"本地撤销"，将 Git 视为"永久历史"

## 另请参阅

* [Interactive mode](/claude-code/08-reference/02-interactive-mode) - 快捷键和会话控制
* [Built-in commands](/claude-code/08-reference/02-interactive-mode#built-in-commands) - 使用 `/rewind` 访问 checkpoints
* [CLI reference](/claude-code/08-reference/01-cli-reference) - 命令行选项
