---
title: "模型提供商快速入门"
description: "模型提供商快速入门 文档"
---

OpenClaw 可以使用许多 LLM 提供商。选择一个，进行认证，然后将默认模型设置为 `provider/model`。

## 推荐：Venice（Venice AI）

Venice 是我们推荐的 Venice AI 设置，用于隐私优先的推理，并可选择使用 Opus 处理最困难的任务。

* 默认：`venice/llama-3.3-70b`
* 最佳综合：`venice/claude-opus-45`（Opus 仍然是最强的）

参见 [Venice AI](/11-providers/venice)。

## 快速开始（两个步骤）

1. 与提供商认证（通常通过 `openclaw onboard`）。
2. 设置默认模型：

```json5  theme={null}
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-5" } } },
}
```

## 支持的提供商（入门集）

* [OpenAI（API + Codex）](/11-providers/openai)
* [Anthropic（API + Claude Code CLI）](/11-providers/anthropic)
* [OpenRouter](/11-providers/openrouter)
* [Vercel AI Gateway](/11-providers/vercel-ai-gateway)
* [Moonshot AI（Kimi + Kimi Coding）](/11-providers/moonshot)
* [Synthetic](/11-providers/synthetic)
* [OpenCode Zen](/11-providers/opencode)
* [Z.AI](/11-providers/zai)
* [GLM 模型](/11-providers/glm)
* [MiniMax](/11-providers/minimax)
* [Venice（Venice AI）](/11-providers/venice)
* [Amazon Bedrock](/11-providers/bedrock)

有关完整的提供商目录（xAI、Groq、Mistral 等）和高级配置，请参阅[模型提供商](/03-concepts/model-providers)。
