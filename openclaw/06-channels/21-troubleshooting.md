---
title: "渠道故障排除"
description: "渠道故障排除 文档"
---

首先运行：

```bash  theme={null}
openclaw doctor
openclaw channels status --probe
```

`channels status --probe` 会在检测到常见渠道配置错误时输出警告，并包含小型实时检查（凭据、部分权限/成员资格）。

## 渠道

* Discord：[/06-channels/discord#troubleshooting](/06-channels/discord#troubleshooting)
* Telegram：[/06-channels/telegram#troubleshooting](/06-channels/telegram#troubleshooting)
* WhatsApp：[/06-channels/whatsapp#troubleshooting-quick](/06-channels/whatsapp#troubleshooting-quick)

## Telegram 快速修复

* 日志显示 `HttpError: Network request for 'sendMessage' failed` 或 `sendChatAction` → 检查 IPv6 DNS。如果 `api.telegram.org` 优先解析为 IPv6 而主机缺少 IPv6 出站连接，请强制使用 IPv4 或启用 IPv6。参见 [/06-channels/telegram#troubleshooting](/06-channels/telegram#troubleshooting)。
* 日志显示 `setMyCommands failed` → 检查到 `api.telegram.org` 的出站 HTTPS 和 DNS 可达性（常见于限制严格的 VPS 或代理环境）。
