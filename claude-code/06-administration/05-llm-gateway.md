---
title: "LLM gateway 配置"
description: "了解如何配置 Claude Code 以使用 LLM gateway 解决方案。涵盖网关要求、身份验证配置、模型选择和特定提供商的端点设置。"
---

LLM gateway 提供了 Claude Code 和模型提供商之间的集中代理层，通常提供以下功能：

* **集中身份验证** - API 密钥管理的单一入口
* **使用情况跟踪** - 监控团队和项目的使用情况
* **成本控制** - 实施预算和速率限制
* **审计日志** - 跟踪所有模型交互以实现合规性
* **模型路由** - 无需更改代码即可在提供商之间切换

## 网关要求

为了使 LLM gateway 与 Claude Code 配合使用，它必须满足以下要求：

**API 格式**

网关必须向客户端公开以下至少一种 API 格式：

1. **Anthropic Messages**: `/v1/messages`, `/v1/messages/count_tokens`
   * 必须转发请求头：`anthropic-beta`、`anthropic-version`

2. **Bedrock InvokeModel**: `/invoke`, `/invoke-with-response-stream`
   * 必须保留请求体字段：`anthropic_beta`、`anthropic_version`

3. **Vertex rawPredict**: `:rawPredict`、`:streamRawPredict`、`/count-tokens:rawPredict`
   * 必须转发请求头：`anthropic-beta`、`anthropic-version`

未能转发请求头或保留请求体字段可能导致功能减少或无法使用 Claude Code 功能。

<Note>
  Claude Code 根据 API 格式确定要启用的功能。当使用 Bedrock 或 Vertex 的 Anthropic Messages 格式时，您可能需要设置环境变量 `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS=1`。
</Note>

## 配置

### 模型选择

默认情况下，Claude Code 将为选定的 API 格式使用标准模型名称。

如果您在网关中配置了自定义模型名称，请使用 [模型配置](/zh-CN/model-config) 中记录的环境变量来匹配您的自定义名称。

## LiteLLM 配置

<Note>
  LiteLLM 是第三方代理服务。Anthropic 不认可、维护或审计 LiteLLM 的安全性或功能。本指南仅供参考，可能会过时。请自行判断使用。
</Note>

### 前置条件

* Claude Code 更新到最新版本
* LiteLLM Proxy Server 已部署且可访问
* 通过您选择的提供商访问 Claude 模型

### 基本 LiteLLM 设置

**配置 Claude Code**：

#### 身份验证方法

##### 静态 API 密钥

使用固定 API 密钥的最简单方法：

```bash  theme={null}
# 在环境中设置
export ANTHROPIC_AUTH_TOKEN=sk-litellm-static-key

# 或在 Claude Code 设置中
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "sk-litellm-static-key"
  }
}
```

此值将作为 `Authorization` 请求头发送。

##### 使用辅助程序的动态 API 密钥

用于轮换密钥或按用户身份验证：

1. 创建 API 密钥辅助程序脚本：

```bash  theme={null}
#!/bin/bash
# ~/bin/get-litellm-key.sh

# 示例：从保险库获取密钥
vault kv get -field=api_key secret/litellm/claude-code

# 示例：生成 JWT 令牌
jwt encode \
  --secret="${JWT_SECRET}" \
  --exp="+1h" \
  '{"user":"'${USER}'","team":"engineering"}'
```

2. 配置 Claude Code 设置以使用辅助程序：

```json  theme={null}
{
  "apiKeyHelper": "~/bin/get-litellm-key.sh"
}
```

3. 设置令牌刷新间隔：

```bash  theme={null}
# 每小时刷新一次（3600000 毫秒）
export CLAUDE_CODE_API_KEY_HELPER_TTL_MS=3600000
```

此值将作为 `Authorization` 和 `X-Api-Key` 请求头发送。`apiKeyHelper` 的优先级低于 `ANTHROPIC_AUTH_TOKEN` 或 `ANTHROPIC_API_KEY`。

#### 统一端点（推荐）

使用 LiteLLM 的 [Anthropic 格式端点](https://docs.litellm.ai/docs/anthropic_unified)：

```bash  theme={null}
export ANTHROPIC_BASE_URL=https://litellm-server:4000
```

**统一端点相对于直通端点的优势：**

* 负载均衡
* 故障转移
* 对成本跟踪和最终用户跟踪的一致支持

#### 特定提供商的直通端点（替代方案）

##### 通过 LiteLLM 的 Claude API

使用 [直通端点](https://docs.litellm.ai/docs/pass_through/anthropic_completion)：

```bash  theme={null}
export ANTHROPIC_BASE_URL=https://litellm-server:4000/anthropic
```

##### 通过 LiteLLM 的 Amazon Bedrock

使用 [直通端点](https://docs.litellm.ai/docs/pass_through/bedrock)：

```bash  theme={null}
export ANTHROPIC_BEDROCK_BASE_URL=https://litellm-server:4000/bedrock
export CLAUDE_CODE_SKIP_BEDROCK_AUTH=1
export CLAUDE_CODE_USE_BEDROCK=1
```

##### 通过 LiteLLM 的 Google Vertex AI

使用 [直通端点](https://docs.litellm.ai/docs/pass_through/vertex_ai)：

```bash  theme={null}
export ANTHROPIC_VERTEX_BASE_URL=https://litellm-server:4000/vertex_ai/v1
export ANTHROPIC_VERTEX_PROJECT_ID=your-gcp-project-id
export CLAUDE_CODE_SKIP_VERTEX_AUTH=1
export CLAUDE_CODE_USE_VERTEX=1
export CLOUD_ML_REGION=us-east5
```

有关更多详细信息，请参阅 [LiteLLM 文档](https://docs.litellm.ai/)。

## 其他资源

* [LiteLLM 文档](https://docs.litellm.ai/)
* [Claude Code 设置](/zh-CN/settings)
* [企业网络配置](/zh-CN/network-config)
* [第三方集成概述](/zh-CN/third-party-integrations)
