---
title: "IRC"
description: "通过 IRC 连接 OpenClaw"
---

# IRC

<Note>
IRC 渠道目前为实验性功能，通过插件提供。
</Note>

## 概述

IRC（Internet Relay Chat）是一个经典的实时聊天协议。OpenClaw 可以通过 IRC 插件连接到 IRC 网络。

## 安装

```bash
openclaw plugins install @openclaw/channel-irc
```

## 配置

在 `~/.openclaw/config.yaml` 中添加：

```yaml
channels:
  irc:
    enabled: true
    server: irc.example.com
    port: 6697
    ssl: true
    nick: openclaw-bot
    channels:
      - "#mychannel"
```

## 环境变量

| 变量 | 描述 |
|------|------|
| `IRC_SERVER` | IRC 服务器地址 |
| `IRC_PORT` | 端口号（默认 6697） |
| `IRC_NICK` | 机器人昵称 |
| `IRC_PASSWORD` | 服务器密码（可选） |

## 支持的功能

* 公共频道消息
* 私信
* 基本格式（粗体、斜体）

## 限制

* 不支持媒体文件
* 消息长度受 IRC 协议限制
* 不支持表情回应

## 相关资源

* [渠道概览](/openclaw/channels/index)
* [故障排除](/openclaw/channels/troubleshooting)
