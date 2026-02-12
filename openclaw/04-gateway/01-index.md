---
title: "Gateway 网关"
description: "OpenClaw Gateway 网关概览"
---

Gateway 是 OpenClaw 的核心服务，负责处理所有消息路由、工具调用和智能体协调。

## 核心功能

* **消息路由** - 在聊天渠道和智能体之间路由消息
* **工具执行** - 安全地执行智能体请求的工具
* **会话管理** - 维护对话上下文和状态
* **认证** - 管理 API 密钥和 OAuth 凭证

## 配置

<CardGroup cols={2}>
  <Card title="配置选项" icon="gear" href="/openclaw/04-gateway/configuration">
    Gateway 配置详解
  </Card>
  <Card title="认证" icon="lock" href="/openclaw/04-gateway/authentication">
    API 密钥和 OAuth 设置
  </Card>
  <Card title="发现" icon="magnifying-glass" href="/openclaw/04-gateway/discovery">
    服务发现机制
  </Card>
  <Card title="协议" icon="code" href="/openclaw/04-gateway/protocol">
    通信协议规范
  </Card>
</CardGroup>

## 运维

<CardGroup cols={2}>
  <Card title="健康检查" icon="heart-pulse" href="/openclaw/04-gateway/health">
    健康状态监控
  </Card>
  <Card title="日志" icon="file-lines" href="/openclaw/04-gateway/logging">
    日志配置和查看
  </Card>
  <Card title="沙盒" icon="shield" href="/openclaw/04-gateway/sandboxing">
    安全沙盒设置
  </Card>
  <Card title="远程访问" icon="globe" href="/openclaw/04-gateway/remote">
    远程 Gateway 配置
  </Card>
</CardGroup>

## 常用命令

```bash
# 启动 Gateway
openclaw gateway start

# 检查状态
openclaw status

# 健康检查
openclaw health

# 查看日志
openclaw logs
```

## 故障排除

遇到问题？查看 [Gateway 故障排除](/openclaw/04-gateway/troubleshooting)。
