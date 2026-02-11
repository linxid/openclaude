---
title: "工具"
description: "OpenClaw 智能体工具"
---

OpenClaw 智能体可以使用各种工具来完成任务。工具是智能体与外部世界交互的主要方式。

## 内置工具

<CardGroup cols={2}>
  <Card title="Skills" icon="wand-magic-sparkles" href="/openclaw/tools/skills">
    可复用的技能包
  </Card>
  <Card title="子智能体" icon="users" href="/openclaw/tools/subagents">
    多智能体协作
  </Card>
  <Card title="浏览器" icon="globe" href="/openclaw/tools/browser">
    网页浏览和自动化
  </Card>
  <Card title="执行" icon="terminal" href="/openclaw/tools/exec">
    命令行执行
  </Card>
</CardGroup>

## 扩展工具

<CardGroup cols={2}>
  <Card title="插件" icon="puzzle-piece" href="/openclaw/tools/plugin">
    第三方插件系统
  </Card>
  <Card title="斜杠命令" icon="slash" href="/openclaw/tools/slash-commands">
    快捷命令
  </Card>
  <Card title="思考" icon="brain" href="/openclaw/tools/thinking">
    思维链工具
  </Card>
  <Card title="Web" icon="network-wired" href="/openclaw/tools/web">
    Web API 调用
  </Card>
</CardGroup>

## 工具安全

工具执行遵循严格的安全策略：

* **沙盒执行** - 工具在隔离环境中运行
* **权限控制** - 用户可以批准或拒绝工具调用
* **审计日志** - 所有工具调用都有记录

## 自定义工具

你可以通过插件系统添加自定义工具。参见[插件开发](/openclaw/tools/plugin)了解详情。
