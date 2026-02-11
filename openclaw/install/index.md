---
title: "安装概览"
description: "OpenClaw 安装指南"
---

# 安装

除非有特殊原因，否则请使用安装器。它会设置 CLI 并运行新手引导。

## 快速安装（推荐）

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

Windows（PowerShell）：

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

下一步（如果你跳过了新手引导）：

```bash
openclaw onboard --install-daemon
```

## 系统要求

* **Node >=22**
* macOS、Linux 或通过 WSL2 的 Windows
* `pnpm` 仅在从源代码构建时需要

## 安装选项

<CardGroup cols={2}>
  <Card title="Node.js" icon="node-js" href="/openclaw/install/node">
    通过 npm 全局安装
  </Card>
  <Card title="Docker" icon="docker" href="/openclaw/install/docker">
    容器化部署
  </Card>
  <Card title="Nix" icon="snowflake" href="/openclaw/install/nix">
    Nix 包管理器
  </Card>
  <Card title="Ansible" icon="gear" href="/openclaw/install/ansible">
    自动化部署
  </Card>
</CardGroup>

## 云平台部署

<CardGroup cols={3}>
  <Card title="Fly.io" href="/openclaw/install/fly">
    Fly.io 部署
  </Card>
  <Card title="Railway" href="/openclaw/install/railway">
    Railway 部署
  </Card>
  <Card title="Render" href="/openclaw/install/render">
    Render 部署
  </Card>
  <Card title="GCP" href="/openclaw/install/gcp">
    Google Cloud
  </Card>
  <Card title="Hetzner" href="/openclaw/install/hetzner">
    Hetzner VPS
  </Card>
</CardGroup>

## 安装后

* 运行新手引导：`openclaw onboard --install-daemon`
* 快速检查：`openclaw doctor`
* 检查 Gateway 健康状态：`openclaw status` + `openclaw health`
* 打开仪表板：`openclaw dashboard`

## 维护

* [更新](/openclaw/install/updating) - 升级到最新版本
* [卸载](/openclaw/install/uninstall) - 完全移除 OpenClaw
