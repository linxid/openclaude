---
title: "GLM 模型"
description: "GLM 模型 文档"
---

GLM 是一个**模型系列**（而非公司），通过 Z.AI 平台提供。在 OpenClaw 中，GLM 模型通过 `zai` 提供商访问，模型 ID 格式如 `zai/glm-4.7`。

## CLI 设置

```bash  theme={null}
openclaw onboard --auth-choice zai-api-key
```

## 配置片段

```json5  theme={null}
{
  env: { ZAI_API_KEY: "sk-..." },
  agents: { defaults: { model: { primary: "zai/glm-4.7" } } },
}
```

## 注意事项

* GLM 版本和可用性可能会变化；请查阅 Z.AI 的文档获取最新信息。
* 示例模型 ID 包括 `glm-4.7` 和 `glm-4.6`。
* 有关提供商的详细信息，请参阅 [/11-providers/zai](/11-providers/zai)。
