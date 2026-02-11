---
title: "使用第三方 API"
description: "配置 Claude Code 使用 Kimi、MiniMax、GLM、DeepSeek 等第三方模型 API。"
---

Claude Code 支持通过 OpenAI 兼容 API 接口使用第三方模型。本文介绍如何配置各种国内外模型提供商。

## 配置方式

Claude Code 支持两种配置第三方 API 的方式：

### 方式一：环境变量

```bash
# 设置 API 基础地址
export ANTHROPIC_BASE_URL="https://api.example.com/v1"

# 设置 API 密钥
export ANTHROPIC_API_KEY="your-api-key"

# 设置模型名称（可选）
export ANTHROPIC_MODEL="model-name"
```

### 方式二：配置文件

在 `~/.claude/settings.json` 中添加：

```json
{
  "apiProvider": "custom",
  "customApiUrl": "https://api.example.com/v1",
  "customApiKey": "your-api-key",
  "customModel": "model-name"
}
```

## 国内模型提供商

### Kimi (月之暗面)

Kimi 2.5 是月之暗面推出的最新模型，支持超长上下文。

```bash
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/v1"
export ANTHROPIC_API_KEY="sk-your-moonshot-key"
export ANTHROPIC_MODEL="moonshot-v1-128k"
```

**可用模型：**
| 模型名称 | 上下文长度 | 说明 |
|---------|-----------|------|
| `moonshot-v1-8k` | 8K | 快速响应 |
| `moonshot-v1-32k` | 32K | 平衡选择 |
| `moonshot-v1-128k` | 128K | 长文本处理 |
| `kimi-latest` | 1M | Kimi 2.5 最新版 |

**获取 API Key：** [https://platform.moonshot.cn](https://platform.moonshot.cn)

---

### MiniMax

MiniMax 提供高性能的国产大模型服务。

```bash
export ANTHROPIC_BASE_URL="https://api.minimax.chat/v1"
export ANTHROPIC_API_KEY="your-minimax-key"
export ANTHROPIC_MODEL="abab6.5s-chat"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `abab6.5s-chat` | 高性能对话模型 |
| `abab6.5g-chat` | 通用对话模型 |
| `abab5.5-chat` | 轻量级模型 |

**获取 API Key：** [https://platform.minimaxi.com](https://platform.minimaxi.com)

---

### 智谱 GLM

智谱 AI 的 GLM 系列模型，包括 GLM-4 等。

```bash
export ANTHROPIC_BASE_URL="https://open.bigmodel.cn/api/paas/v4"
export ANTHROPIC_API_KEY="your-zhipu-key"
export ANTHROPIC_MODEL="glm-4-plus"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `glm-4-plus` | 最新旗舰模型 |
| `glm-4` | 高性能模型 |
| `glm-4-air` | 快速响应 |
| `glm-4-airx` | 极速响应 |
| `glm-4-flash` | 免费模型 |

**获取 API Key：** [https://open.bigmodel.cn](https://open.bigmodel.cn)

---

### DeepSeek

DeepSeek 提供高性价比的代码和推理模型。

```bash
export ANTHROPIC_BASE_URL="https://api.deepseek.com/v1"
export ANTHROPIC_API_KEY="sk-your-deepseek-key"
export ANTHROPIC_MODEL="deepseek-chat"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `deepseek-chat` | 通用对话模型 |
| `deepseek-coder` | 代码专用模型 |
| `deepseek-reasoner` | 推理增强模型 |

**获取 API Key：** [https://platform.deepseek.com](https://platform.deepseek.com)

---

### 百度千帆

百度文心大模型平台。

```bash
export ANTHROPIC_BASE_URL="https://aip.baidubce.com/rpc/2.0/ai_custom/v1/wenxinworkshop"
export ANTHROPIC_API_KEY="your-access-token"
export ANTHROPIC_MODEL="ernie-4.0-8k"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `ernie-4.0-8k` | ERNIE 4.0 |
| `ernie-3.5-8k` | ERNIE 3.5 |
| `ernie-speed-128k` | 长文本快速 |

**获取 API Key：** [https://console.bce.baidu.com/qianfan](https://console.bce.baidu.com/qianfan)

---

### 阿里通义千问

阿里云通义千问系列模型。

```bash
export ANTHROPIC_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"
export ANTHROPIC_API_KEY="sk-your-dashscope-key"
export ANTHROPIC_MODEL="qwen-max"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `qwen-max` | 最强能力 |
| `qwen-plus` | 平衡选择 |
| `qwen-turbo` | 快速响应 |
| `qwen-long` | 长文本 |

**获取 API Key：** [https://dashscope.console.aliyun.com](https://dashscope.console.aliyun.com)

---

## 国际模型提供商

### OpenRouter

OpenRouter 聚合了多种模型，一个 API 访问所有模型。

```bash
export ANTHROPIC_BASE_URL="https://openrouter.ai/api/v1"
export ANTHROPIC_API_KEY="sk-or-your-key"
export ANTHROPIC_MODEL="anthropic/claude-3.5-sonnet"
```

**热门模型：**
| 模型名称 | 说明 |
|---------|------|
| `anthropic/claude-3.5-sonnet` | Claude 3.5 Sonnet |
| `openai/gpt-4-turbo` | GPT-4 Turbo |
| `google/gemini-pro-1.5` | Gemini 1.5 Pro |
| `meta-llama/llama-3.1-405b` | Llama 3.1 405B |

**获取 API Key：** [https://openrouter.ai](https://openrouter.ai)

---

### Groq

Groq 提供超快速的推理服务。

```bash
export ANTHROPIC_BASE_URL="https://api.groq.com/openai/v1"
export ANTHROPIC_API_KEY="gsk_your-groq-key"
export ANTHROPIC_MODEL="llama-3.1-70b-versatile"
```

**可用模型：**
| 模型名称 | 说明 |
|---------|------|
| `llama-3.1-70b-versatile` | Llama 3.1 70B |
| `llama-3.1-8b-instant` | Llama 3.1 8B |
| `mixtral-8x7b-32768` | Mixtral 8x7B |

**获取 API Key：** [https://console.groq.com](https://console.groq.com)

---

### Together AI

Together AI 提供开源模型的托管服务。

```bash
export ANTHROPIC_BASE_URL="https://api.together.xyz/v1"
export ANTHROPIC_API_KEY="your-together-key"
export ANTHROPIC_MODEL="meta-llama/Llama-3-70b-chat-hf"
```

**获取 API Key：** [https://api.together.xyz](https://api.together.xyz)

---

## 自托管方案

### Ollama (本地部署)

使用 Ollama 在本地运行开源模型。

```bash
# 启动 Ollama 服务
ollama serve

# 配置 Claude Code
export ANTHROPIC_BASE_URL="http://localhost:11434/v1"
export ANTHROPIC_API_KEY="ollama"  # Ollama 不需要真实 key
export ANTHROPIC_MODEL="llama3.1:70b"
```

**推荐模型：**
- `llama3.1:70b` - 最强开源模型
- `codellama:34b` - 代码专用
- `qwen2.5:72b` - 通义千问开源版
- `deepseek-coder-v2:16b` - DeepSeek 代码模型

---

### vLLM / Text Generation Inference

自建推理服务：

```bash
export ANTHROPIC_BASE_URL="http://your-server:8000/v1"
export ANTHROPIC_API_KEY="your-key"
export ANTHROPIC_MODEL="your-model"
```

---

## 常见问题

### 如何验证配置是否正确？

运行以下命令测试连接：

```bash
claude --version
claude "你好，请介绍一下你自己"
```

### 模型不支持某些功能怎么办？

部分第三方模型可能不支持 Claude Code 的所有功能（如工具调用）。建议：
- 使用支持 function calling 的模型
- 查看模型提供商的 API 文档了解支持的功能

### 如何切换不同的模型？

可以通过修改环境变量或使用配置文件快速切换：

```bash
# 切换到 Kimi
export ANTHROPIC_MODEL="moonshot-v1-128k"

# 切换到 DeepSeek
export ANTHROPIC_MODEL="deepseek-chat"
```

### API 调用失败怎么办？

1. 检查 API Key 是否正确
2. 检查网络连接
3. 确认 API 余额是否充足
4. 查看错误信息了解具体原因

---

## 相关资源

- [模型配置](/claude-code/07-configuration/04-model-config) - 更多模型配置选项
- [网络配置](/claude-code/07-configuration/07-network-config) - 代理和网络设置
- [第三方集成](/claude-code/06-administration/01-third-party-integrations) - 企业级集成
