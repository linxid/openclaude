---
title: "文档导航中心"
description: "文档导航中心 文档"
---

使用这些导航中心发现每一个页面，包括深入解析和参考文档——它们不一定出现在左侧导航栏中。

## 从这里开始

* [索引](/)
* [入门指南](/01-start/getting-started)
* [快速开始](/01-start/quickstart)
* [新手引导](/01-start/onboarding)
* [向导](/01-start/wizard)
* [安装配置](/01-start/setup)
* [仪表盘（本地 Gateway 网关）](http://127.0.0.1:18789/)
* [帮助](/help)
* [文档目录](/01-start/docs-directory)
* [配置](/04-gateway/configuration)
* [配置示例](/04-gateway/configuration-examples)
* [OpenClaw 助手](/01-start/openclaw)
* [展示](/01-start/showcase)
* [背景故事](/01-start/lore)

## 安装 + 更新

* [Docker](/02-install/docker)
* [Nix](/02-install/nix)
* [更新 / 回滚](/02-install/updating)
* [Bun 工作流（实验性）](/02-install/bun)

## 核心概念

* [架构](/03-concepts/architecture)
* [功能](/03-concepts/features)
* [网络中心](/network)
* [智能体运行时](/03-concepts/agent)
* [智能体工作区](/03-concepts/agent-workspace)
* [记忆](/03-concepts/memory)
* [智能体循环](/03-concepts/agent-loop)
* [流式传输 + 分块](/03-concepts/streaming)
* [多智能体路由](/03-concepts/multi-agent)
* [压缩](/03-concepts/compaction)
* [会话](/03-concepts/session)
* [会话（别名）](/03-concepts/sessions)
* [会话修剪](/03-concepts/session-pruning)
* [会话工具](/03-concepts/session-tool)
* [队列](/03-concepts/queue)
* [斜杠命令](/07-tools/slash-commands)
* [RPC 适配器](/17-reference/rpc)
* [TypeBox 模式](/03-concepts/typebox)
* [时区处理](/03-concepts/timezone)
* [在线状态](/03-concepts/presence)
* [设备发现 + 传输协议](/04-gateway/discovery)
* [Bonjour](/04-gateway/bonjour)
* [渠道路由](/06-channels/channel-routing)
* [群组](/06-channels/groups)
* [群组消息](/06-channels/group-messages)
* [模型故障转移](/03-concepts/model-failover)
* [OAuth](/03-concepts/oauth)

## 提供商 + 入口

* [聊天渠道中心](/channels)
* [模型提供商中心](/11-providers/models)
* [WhatsApp](/06-channels/whatsapp)
* [Telegram](/06-channels/telegram)
* [Telegram（grammY 注意事项）](/06-channels/grammy)
* [Slack](/06-channels/slack)
* [Discord](/06-channels/discord)
* [Mattermost](/06-channels/mattermost)（插件）
* [Signal](/06-channels/signal)
* [BlueBubbles (iMessage)](/06-channels/bluebubbles)
* [iMessage（旧版）](/06-channels/imessage)
* [位置解析](/06-channels/location)
* [WebChat](/14-web/webchat)
* [Webhooks](/08-automation/webhook)
* [Gmail Pub/Sub](/08-automation/gmail-pubsub)

## Gateway 网关 + 运维

* [Gateway 网关运维手册](/gateway)
* [网络模型](/04-gateway/network-model)
* [Gateway 网关配对](/04-gateway/pairing)
* [Gateway 网关锁](/04-gateway/gateway-lock)
* [后台进程](/04-gateway/background-process)
* [健康检查](/04-gateway/health)
* [心跳](/04-gateway/heartbeat)
* [Doctor](/04-gateway/doctor)
* [日志](/04-gateway/logging)
* [沙箱隔离](/04-gateway/sandboxing)
* [仪表盘](/14-web/dashboard)
* [控制界面](/14-web/control-ui)
* [远程访问](/04-gateway/remote)
* [远程 Gateway 网关 README](/04-gateway/remote-gateway-readme)
* [Tailscale](/04-gateway/tailscale)
* [安全](/04-gateway/security)
* [故障排除](/04-gateway/troubleshooting)

## 工具 + 自动化

* [工具概览](/tools)
* [OpenProse](/prose)
* [CLI 参考](/cli)
* [Exec 工具](/07-tools/exec)
* [提权模式](/07-tools/elevated)
* [定时任务](/08-automation/cron-jobs)
* [定时任务 vs 心跳](/08-automation/cron-vs-heartbeat)
* [思考 + 详细输出](/07-tools/thinking)
* [模型](/03-concepts/models)
* [子智能体](/07-tools/subagents)
* [Agent send CLI](/07-tools/agent-send)
* [终端界面](/14-web/tui)
* [浏览器控制](/07-tools/browser)
* [浏览器（Linux 故障排除）](/07-tools/browser-linux-troubleshooting)
* [轮询](/08-automation/poll)

## 节点、媒体、语音

* [节点概览](/nodes)
* [摄像头](/09-nodes/camera)
* [图片](/09-nodes/images)
* [音频](/09-nodes/audio)
* [位置命令](/09-nodes/location-command)
* [语音唤醒](/09-nodes/voicewake)
* [对话模式](/09-nodes/talk)

## 平台

* [平台概览](/platforms)
* [macOS](/10-platforms/macos)
* [iOS](/10-platforms/ios)
* [Android](/10-platforms/android)
* [Windows (WSL2)](/10-platforms/windows)
* [Linux](/10-platforms/linux)
* [Web 界面](/web)

## macOS 配套应用（高级）

* [macOS 开发环境配置](/10-platforms/mac/dev-setup)
* [macOS 菜单栏](/10-platforms/mac/menu-bar)
* [macOS 语音唤醒](/10-platforms/mac/voicewake)
* [macOS 语音悬浮窗](/10-platforms/mac/voice-overlay)
* [macOS WebChat](/10-platforms/mac/webchat)
* [macOS Canvas](/10-platforms/mac/canvas)
* [macOS 子进程](/10-platforms/mac/child-process)
* [macOS 健康检查](/10-platforms/mac/health)
* [macOS 图标](/10-platforms/mac/icon)
* [macOS 日志](/10-platforms/mac/logging)
* [macOS 权限](/10-platforms/mac/permissions)
* [macOS 远程](/10-platforms/mac/remote)
* [macOS 签名](/10-platforms/mac/signing)
* [macOS 发布](/10-platforms/mac/release)
* [macOS Gateway 网关 (launchd)](/10-platforms/mac/bundled-gateway)
* [macOS XPC](/10-platforms/mac/xpc)
* [macOS Skills](/10-platforms/mac/skills)
* [macOS Peekaboo](/10-platforms/mac/peekaboo)

## 工作区 + 模板

* [Skills](/07-tools/skills)
* [ClawHub](/07-tools/clawhub)
* [Skills 配置](/07-tools/skills-config)
* [默认 AGENTS](/17-reference/AGENTS.default)
* [模板：AGENTS](/17-reference/templates/AGENTS)
* [模板：BOOTSTRAP](/17-reference/templates/BOOTSTRAP)
* [模板：HEARTBEAT](/17-reference/templates/HEARTBEAT)
* [模板：IDENTITY](/17-reference/templates/IDENTITY)
* [模板：SOUL](/17-reference/templates/SOUL)
* [模板：TOOLS](/17-reference/templates/TOOLS)
* [模板：USER](/17-reference/templates/USER)

## 实验（探索性）

* [新手引导配置协议](/16-experiments/onboarding-config-protocol)
* [定时任务加固笔记](/16-experiments/plans/cron-add-hardening)
* [群组策略加固笔记](/16-experiments/plans/group-policy-hardening)
* [研究：记忆](/16-experiments/research/memory)
* [模型配置探索](/16-experiments/proposals/model-config)

## 项目

* [致谢](/17-reference/credits)

## 测试 + 发布

* [测试](/17-reference/test)
* [发布检查清单](/17-reference/RELEASING)
* [设备型号](/17-reference/device-models)
