---
title: "模型提供商"
description: "模型提供商 文档"
---

OpenClaw 可以使用许多 LLM 提供商。选择一个提供商，进行认证，然后将默认模型设置为 `provider/model`。

正在寻找聊天渠道文档（WhatsApp/Telegram/Discord/Slack/Mattermost（插件）等）？参见[渠道](/channels)。

## 亮点：Venice（Venice AI）

Venice 是我们推荐的 Venice AI 设置，用于隐私优先的推理，并可选择使用 Opus 处理困难任务。

* 默认：`venice/llama-3.3-70b`
* 最佳综合：`venice/claude-opus-45`（Opus 仍然是最强的）

参见 [Venice AI](/11-providers/venice)。

## 快速开始

1. 与提供商进行认证（通常通过 `openclaw onboard`）。
2. 设置默认模型：

```json5  theme={null}
{
  agents: { defaults: { model: { primary: "anthropic/claude-opus-4-5" } } },
}
```

## 提供商文档

* [OpenAI（API + Codex）](/11-providers/openai)
* [Anthropic（API + Claude Code CLI）](/11-providers/anthropic)
* [Qwen（OAuth）](/11-providers/qwen)
* [OpenRouter](/11-providers/openrouter)
* [Vercel AI Gateway](/11-providers/vercel-ai-gateway)
* [Moonshot AI（Kimi + Kimi Coding）](/11-providers/moonshot)
* [OpenCode Zen](/11-providers/opencode)
* [Amazon Bedrock](/11-providers/bedrock)
* [Z.AI](/11-providers/zai)
* [Xiaomi](/11-providers/xiaomi)
* [GLM 模型](/11-providers/glm)
* [MiniMax](/11-providers/minimax)
* [Venice（Venice AI，注重隐私）](/11-providers/venice)
* [Ollama（本地模型）](/11-providers/ollama)

## 转录提供商

* [Deepgram（音频转录）](/11-providers/deepgram)

## 社区工具

* [Claude Max API Proxy](/11-providers/claude-max-api-proxy) - 将 Claude Max/Pro 订阅作为 OpenAI 兼容的 API 端点使用

有关完整的提供商目录（xAI、Groq、Mistral 等）和高级配置，
参见[模型提供商](/03-concepts/model-providers)。
