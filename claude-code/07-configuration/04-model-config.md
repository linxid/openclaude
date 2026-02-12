---
title: "模型配置"
description: "了解 Claude Code 模型配置，包括模型别名如 `opusplan`"
---

## 可用模型

对于 Claude Code 中的 `model` 设置，你可以配置以下任一项：

* 一个**模型别名**
* 一个**模型名称**
  * Anthropic API：完整的\*\*[模型名称](https://platform.claude.com/docs/en/about-claude/models/overview)\*\*
  * Bedrock：推理配置文件 ARN
  * Foundry：部署名称
  * Vertex：版本名称

### 模型别名

模型别名提供了一种便捷的方式来选择模型设置，无需记住确切的版本号：

| 模型别名             | 行为                                                                                                                                 |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| **`default`**    | 推荐的模型设置，取决于你的账户类型                                                                                                                  |
| **`sonnet`**     | 使用最新的 Sonnet 模型（当前为 Sonnet 4.5）用于日常编码任务                                                                                            |
| **`opus`**       | 使用最新的 Opus 模型（当前为 Opus 4.6）用于复杂推理任务                                                                                                |
| **`haiku`**      | 使用快速高效的 Haiku 模型用于简单任务                                                                                                             |
| **`sonnet[1m]`** | 使用 Sonnet 和[100 万 token 上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows#1m-token-context-window)用于长会话 |
| **`opusplan`**   | 特殊模式，在计划模式中使用 `opus`，然后在执行时切换到 `sonnet`                                                                                            |

别名始终指向最新版本。要固定到特定版本，请使用完整模型名称（例如 `claude-opus-4-5-20251101`）或设置相应的环境变量，如 `ANTHROPIC_DEFAULT_OPUS_MODEL`。

### 设置你的模型

你可以通过多种方式配置模型，按优先级顺序列出：

1. **在会话期间** - 使用 `/model <alias|name>` 在会话中切换模型
2. **启动时** - 使用 `claude --model <alias|name>` 启动
3. **环境变量** - 设置 `ANTHROPIC_MODEL=<alias|name>`
4. **设置** - 在设置文件中使用 `model` 字段永久配置。

使用示例：

```bash  theme={null}
# 使用 Opus 启动
claude --model opus

# 在会话期间切换到 Sonnet
/model sonnet
```

设置文件示例：

```
{
    "permissions": {
        ...
    },
    "model": "opus"
}
```

## 特殊模型行为

### `default` 模型设置

`default` 的行为取决于你的账户类型：

* **Max 和 Teams**：默认为 Opus 4.6
* **Pro**：在 Claude Code 中默认为 Opus 4.6
* **Enterprise**：Opus 4.6 可用但不是默认值

如果你在使用 Opus 时达到使用阈值，Claude Code 可能会自动回退到 Sonnet。

### `opusplan` 模型设置

`opusplan` 模型别名提供了一种自动化的混合方法：

* **在计划模式中** - 使用 `opus` 进行复杂推理和架构决策
* **在执行模式中** - 自动切换到 `sonnet` 进行代码生成和实现

这给你两全其美：Opus 的优越推理能力用于规划，Sonnet 的效率用于执行。

### 调整努力级别

[努力级别](https://platform.claude.com/docs/en/build-with-claude/effort)控制 Opus 4.6 的自适应推理，它根据任务复杂性动态分配思考。较低的努力级别对于直接任务更快更便宜，而较高的努力级别为复杂问题提供更深入的推理。

有三个级别可用：**低**、**中** 和 **高**（默认）。

**设置努力级别：**

* **在 `/model` 中**：选择模型时使用左右箭头键调整努力级别滑块
* **环境变量**：设置 `CLAUDE_CODE_EFFORT_LEVEL=low|medium|high`
* **设置**：在设置文件中设置 `effortLevel`

努力级别目前在 Opus 4.6 上受支持。当选择受支持的模型时，努力级别滑块会出现在 `/model` 中。

### 使用 \[1m] 扩展上下文

`[1m]` 后缀为长会话启用[100 万 token 上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows#1m-token-context-window)。

<Note>
  对于 Opus 4.6，100 万 token 上下文窗口可供 API 和 Claude Code 按使用量付费用户使用。Pro、Max、Teams 和 Enterprise 订阅用户在推出时无法访问 Opus 4.6 100 万 token 上下文。
</Note>

你可以将 `[1m]` 后缀与模型别名或完整模型名称一起使用：

```bash  theme={null}
# 使用 sonnet[1m] 别名
/model sonnet[1m]

# 或将 [1m] 附加到完整模型名称
/model claude-sonnet-4-5-20250929[1m]
```

注意：扩展上下文模型有[不同的定价](https://platform.claude.com/docs/en/about-claude/pricing#long-context-pricing)。

## 检查你当前的模型

你可以通过多种方式查看你当前使用的模型：

1. 在[状态行](/claude-code/07-configuration/09-statusline)中（如果已配置）
2. 在 `/status` 中，它也显示你的账户信息。

## 环境变量

你可以使用以下环境变量，这些变量必须是完整的**模型名称**（或你的 API 提供商的等效项），以控制别名映射到的模型名称。

| 环境变量                             | 描述                                                          |
| -------------------------------- | ----------------------------------------------------------- |
| `ANTHROPIC_DEFAULT_OPUS_MODEL`   | 用于 `opus` 的模型，或在 Plan Mode 活跃时用于 `opusplan` 的模型。            |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | 用于 `sonnet` 的模型，或在 Plan Mode 不活跃时用于 `opusplan` 的模型。         |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL`  | 用于 `haiku` 的模型，或[后台功能](/claude-code/06-administration/08-costs#background-token-usage) |
| `CLAUDE_CODE_SUBAGENT_MODEL`     | 用于 [subagents](/claude-code/04-build-with-claude/02-sub-agents) 的模型                       |

注意：`ANTHROPIC_SMALL_FAST_MODEL` 已弃用，改为使用 `ANTHROPIC_DEFAULT_HAIKU_MODEL`。

### Prompt caching 配置

Claude Code 自动使用 [prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 来优化性能并降低成本。你可以全局禁用 prompt caching 或针对特定模型层禁用：

| 环境变量                            | 描述                                        |
| ------------------------------- | ----------------------------------------- |
| `DISABLE_PROMPT_CACHING`        | 设置为 `1` 以禁用所有模型的 prompt caching（优先于按模型设置） |
| `DISABLE_PROMPT_CACHING_HAIKU`  | 设置为 `1` 以仅禁用 Haiku 模型的 prompt caching     |
| `DISABLE_PROMPT_CACHING_SONNET` | 设置为 `1` 以仅禁用 Sonnet 模型的 prompt caching    |
| `DISABLE_PROMPT_CACHING_OPUS`   | 设置为 `1` 以仅禁用 Opus 模型的 prompt caching      |

这些环境变量为你提供了对 prompt caching 行为的细粒度控制。全局 `DISABLE_PROMPT_CACHING` 设置优先于特定模型设置，允许你在需要时快速禁用所有缓存。按模型设置对于选择性控制很有用，例如在调试特定模型或与可能有不同缓存实现的云提供商合作时。
