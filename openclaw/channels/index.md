---
title: "聊天渠道"
description: "OpenClaw 支持的聊天平台"
---

OpenClaw 可以在你已经使用的任何聊天应用上与你交流。每个渠道通过 Gateway 网关连接。
所有渠道都支持文本；媒体和表情回应的支持因渠道而异。

## 支持的渠道

<CardGroup cols={2}>
  <Card title="Telegram" icon="telegram" href="/openclaw/channels/telegram">
    通过 grammY 使用 Bot API；支持群组
  </Card>
  <Card title="WhatsApp" icon="whatsapp" href="/openclaw/channels/whatsapp">
    最受欢迎；使用 Baileys，需要二维码配对
  </Card>
  <Card title="Discord" icon="discord" href="/openclaw/channels/discord">
    Discord Bot API + Gateway；支持服务器、频道和私信
  </Card>
  <Card title="Slack" icon="slack" href="/openclaw/channels/slack">
    Bolt SDK；工作区应用
  </Card>
  <Card title="Microsoft Teams" icon="microsoft" href="/openclaw/channels/msteams">
    Bot Framework；企业支持
  </Card>
  <Card title="飞书" icon="message" href="/openclaw/channels/feishu">
    飞书（Lark）机器人
  </Card>
  <Card title="LINE" icon="line" href="/openclaw/channels/line">
    LINE Messaging API 机器人
  </Card>
  <Card title="Signal" icon="signal-messenger" href="/openclaw/channels/signal">
    signal-cli；注重隐私
  </Card>
  <Card title="Matrix" icon="hashtag" href="/openclaw/channels/matrix">
    Matrix 协议
  </Card>
</CardGroup>

## 注意事项

* 渠道可以同时运行；配置多个渠道后，OpenClaw 会按聊天进行路由
* 最快的设置方式通常是 **Telegram**（简单的机器人令牌）
* WhatsApp 需要二维码配对，并在磁盘上存储更多状态
* 群组行为因渠道而异；参见[群组](/openclaw/channels/groups)
* 为安全起见，私信配对和允许列表会被强制执行

## 相关资源

* [群组消息](/openclaw/channels/groups) - 群组聊天配置
* [渠道配对](/openclaw/channels/pairing) - 配对设置
* [故障排除](/openclaw/channels/troubleshooting) - 常见问题解决
