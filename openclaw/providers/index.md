---
title: "模型提供商"
description: "OpenClaw 支持的 AI 模型提供商"
---

# 模型提供商

OpenClaw 支持多种 AI 模型提供商，你可以根据需求选择最适合的模型。

## 主要提供商

<CardGroup cols={2}>
  <Card title="Anthropic" icon="a" href="/openclaw/providers/anthropic">
    Claude 系列模型（推荐）
  </Card>
  <Card title="OpenAI" icon="robot" href="/openclaw/providers/openai">
    GPT 系列模型
  </Card>
  <Card title="Amazon Bedrock" icon="aws" href="/openclaw/providers/bedrock">
    AWS 托管模型
  </Card>
  <Card title="OpenRouter" icon="route" href="/openclaw/providers/openrouter">
    多模型聚合
  </Card>
</CardGroup>

## 国内提供商

<CardGroup cols={2}>
  <Card title="MiniMax" href="/openclaw/providers/minimax">
    MiniMax 模型
  </Card>
  <Card title="Moonshot" href="/openclaw/providers/moonshot">
    月之暗面 Kimi
  </Card>
  <Card title="GLM" href="/openclaw/providers/glm">
    智谱 GLM 模型
  </Card>
  <Card title="千帆" href="/openclaw/providers/qianfan">
    百度千帆平台
  </Card>
</CardGroup>

## 配置模型

```bash
# 交互式配置
openclaw configure

# 查看可用模型
openclaw models list

# 设置默认模型
openclaw models set-default claude-sonnet-4-20250514
```

## 模型选择建议

| 用途 | 推荐模型 |
|------|---------|
| 日常对话 | Claude Sonnet |
| 复杂推理 | Claude Opus |
| 快速响应 | Claude Haiku |
| 代码生成 | Claude Sonnet / GPT-4 |

## 相关资源

* [模型列表](/openclaw/providers/models) - 所有支持的模型
* [模型配置](/openclaw/cli/models) - CLI 模型命令
