---
title: "CLI 命令"
description: "OpenClaw 命令行工具"
---

OpenClaw CLI 提供了管理智能体、渠道和 Gateway 的完整命令集。

## 安装 CLI

```bash
npm install -g openclaw@latest
```

## 常用命令

### 智能体管理

```bash
openclaw agent          # 智能体操作
openclaw agents         # 列出所有智能体
```

### 渠道管理

```bash
openclaw channels       # 管理聊天渠道
openclaw pairing        # 渠道配对
```

### Gateway 操作

```bash
openclaw gateway        # Gateway 管理
openclaw health         # 健康检查
openclaw status         # 状态查看
openclaw logs           # 日志查看
```

### 配置

```bash
openclaw configure      # 交互式配置
openclaw setup          # 初始设置
openclaw models         # 模型配置
```

## 命令参考

<CardGroup cols={2}>
  <Card title="setup" href="/openclaw/05-cli/setup">
    初始设置向导
  </Card>
  <Card title="agent" href="/openclaw/05-cli/agent">
    智能体操作
  </Card>
  <Card title="agents" href="/openclaw/05-cli/agents">
    智能体列表
  </Card>
  <Card title="channels" href="/openclaw/05-cli/channels">
    渠道管理
  </Card>
  <Card title="configure" href="/openclaw/05-cli/configure">
    配置选项
  </Card>
  <Card title="gateway" href="/openclaw/05-cli/gateway">
    Gateway 管理
  </Card>
  <Card title="health" href="/openclaw/05-cli/health">
    健康检查
  </Card>
  <Card title="logs" href="/openclaw/05-cli/logs">
    日志查看
  </Card>
  <Card title="memory" href="/openclaw/05-cli/memory">
    记忆管理
  </Card>
  <Card title="message" href="/openclaw/05-cli/message">
    发送消息
  </Card>
  <Card title="models" href="/openclaw/05-cli/models">
    模型配置
  </Card>
  <Card title="skills" href="/openclaw/05-cli/skills">
    技能管理
  </Card>
  <Card title="status" href="/openclaw/05-cli/status">
    状态查看
  </Card>
</CardGroup>

## 获取帮助

```bash
openclaw --help         # 显示所有命令
openclaw <command> -h   # 特定命令帮助
```
