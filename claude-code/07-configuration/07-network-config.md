---
title: "企业网络配置"
description: "为企业环境配置 Claude Code，支持代理服务器、自定义证书颁发机构 (CA) 和相互传输层安全 (mTLS) 身份验证。"
---

# 企业网络配置

> 为企业环境配置 Claude Code，支持代理服务器、自定义证书颁发机构 (CA) 和相互传输层安全 (mTLS) 身份验证。

Claude Code 通过环境变量支持各种企业网络和安全配置。这包括通过公司代理服务器路由流量、信任自定义证书颁发机构 (CA) 以及使用相互传输层安全 (mTLS) 证书进行身份验证以增强安全性。

<Note>
  本页面显示的所有环境变量也可以在 [`settings.json`](/zh-CN/settings) 中配置。
</Note>

## 代理配置

### 环境变量

Claude Code 遵守标准代理环境变量：

```bash  theme={null}
# HTTPS 代理（推荐）
export HTTPS_PROXY=https://proxy.example.com:8080

# HTTP 代理（如果 HTTPS 不可用）
export HTTP_PROXY=http://proxy.example.com:8080

# 绕过特定请求的代理 - 空格分隔格式
export NO_PROXY="localhost 192.168.1.1 example.com .example.com"
# 绕过特定请求的代理 - 逗号分隔格式
export NO_PROXY="localhost,192.168.1.1,example.com,.example.com"
# 绕过所有请求的代理
export NO_PROXY="*"
```

<Note>
  Claude Code 不支持 SOCKS 代理。
</Note>

### 基本身份验证

如果您的代理需要基本身份验证，请在代理 URL 中包含凭据：

```bash  theme={null}
export HTTPS_PROXY=http://username:password@proxy.example.com:8080
```

<Warning>
  避免在脚本中硬编码密码。改用环境变量或安全凭据存储。
</Warning>

<Tip>
  对于需要高级身份验证（NTLM、Kerberos 等）的代理，请考虑使用支持您的身份验证方法的 LLM 网关服务。
</Tip>

## 自定义 CA 证书

如果您的企业环境使用自定义 CA 进行 HTTPS 连接（无论是通过代理还是直接 API 访问），请配置 Claude Code 以信任它们：

```bash  theme={null}
export NODE_EXTRA_CA_CERTS=/path/to/ca-cert.pem
```

## mTLS 身份验证

对于需要客户端证书身份验证的企业环境：

```bash  theme={null}
# 用于身份验证的客户端证书
export CLAUDE_CODE_CLIENT_CERT=/path/to/client-cert.pem

# 客户端私钥
export CLAUDE_CODE_CLIENT_KEY=/path/to/client-key.pem

# 可选：加密私钥的密码短语
export CLAUDE_CODE_CLIENT_KEY_PASSPHRASE="your-passphrase"
```

## 网络访问要求

Claude Code 需要访问以下 URL：

* `api.anthropic.com` - Claude API 端点
* `claude.ai` - WebFetch 保护措施
* `statsig.anthropic.com` - 遥测和指标
* `sentry.io` - 错误报告

确保这些 URL 在您的代理配置和防火墙规则中被列入白名单。在容器化或受限网络环境中使用 Claude Code 时，这一点尤为重要。

## 其他资源

* [Claude Code 设置](/zh-CN/settings)
* [环境变量参考](/zh-CN/settings#environment-variables)
* [故障排除指南](/zh-CN/troubleshooting)
