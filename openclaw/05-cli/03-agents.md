---
title: "agents"
description: "agents 文档"
---

管理隔离的智能体（工作区 + 认证 + 路由）。

相关内容：

* 多智能体路由：[多智能体路由](/03-concepts/multi-agent)
* 智能体工作区：[智能体工作区](/03-concepts/agent-workspace)

## 示例

```bash  theme={null}
openclaw agents list
openclaw agents add work --workspace ~/.openclaw/workspace-work
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
openclaw agents set-identity --agent main --avatar avatars/openclaw.png
openclaw agents delete work
```

## 身份文件

每个智能体工作区可以在工作区根目录包含一个 `IDENTITY.md`：

* 示例路径：`~/.openclaw/workspace/IDENTITY.md`
* `set-identity --from-identity` 从工作区根目录读取（或从显式指定的 `--identity-file` 读取）

头像路径相对于工作区根目录解析。

## 设置身份

`set-identity` 将字段写入 `agents.list[].identity`：

* `name`
* `theme`
* `emoji`
* `avatar`（工作区相对路径、http(s) URL 或 data URI）

从 `IDENTITY.md` 加载：

```bash  theme={null}
openclaw agents set-identity --workspace ~/.openclaw/workspace --from-identity
```

显式覆盖字段：

```bash  theme={null}
openclaw agents set-identity --agent main --name "OpenClaw" --emoji "🦞" --avatar avatars/openclaw.png
```

配置示例：

```json5  theme={null}
{
  agents: {
    list: [
      {
        id: "main",
        identity: {
          name: "OpenClaw",
          theme: "space lobster",
          emoji: "🦞",
          avatar: "avatars/openclaw.png",
        },
      },
    ],
  },
}
```
