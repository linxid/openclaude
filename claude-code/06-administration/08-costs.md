---
title: "有效管理成本"
description: "了解如何在使用 Claude Code 时跟踪和优化令牌使用情况和成本。"
---

# 有效管理成本

> 了解如何在使用 Claude Code 时跟踪和优化令牌使用情况和成本。

Claude Code 每次交互都会消耗令牌。平均成本为每个开发者每天 $6，90% 的用户每日成本保持在 $12 以下。

对于团队使用，Claude Code 按 API 令牌消耗收费。平均而言，Claude Code 使用 Sonnet 4.5 的成本约为每个开发者每月 \$100-200，但根据用户运行的实例数量以及是否在自动化中使用，成本差异很大。

## 跟踪您的成本

### 使用 `/cost` 命令

<Note>
  `/cost` 命令不适用于 Claude Max 和 Pro 订阅者。
</Note>

`/cost` 命令为您当前的会话提供详细的令牌使用统计信息：

```
Total cost:            $0.55
Total duration (API):  6m 19.7s
Total duration (wall): 6h 33m 10.2s
Total code changes:    0 lines added, 0 lines removed
```

### 其他跟踪选项

在 Claude Console 中检查[历史使用情况](https://support.claude.com/zh-CN/articles/9534590-cost-and-usage-reporting-in-console)（需要管理员或计费角色）并为 Claude Code 工作区设置[工作区支出限制](https://support.claude.com/zh-CN/articles/9796807-creating-and-managing-workspaces)（需要管理员角色）。

<Note>
  当您首次使用 Claude Console 账户对 Claude Code 进行身份验证时，会自动为您创建一个名为"Claude Code"的工作区。此工作区为您的组织中的所有 Claude Code 使用提供集中式成本跟踪和管理。您无法为此工作区创建 API 密钥 - 它专门用于 Claude Code 身份验证和使用。
</Note>

## 为团队管理成本

使用 Claude API 时，您可以限制 Claude Code 工作区的总支出。要进行配置，请[按照这些说明](https://support.claude.com/zh-CN/articles/9796807-creating-and-managing-workspaces)操作。管理员可以通过[按照这些说明](https://support.claude.com/zh-CN/articles/9534590-cost-and-usage-reporting-in-console)查看成本和使用情况报告。

在 Bedrock 和 Vertex 上，Claude Code 不会从您的云中发送指标。为了获取成本指标，一些大型企业报告使用了 [LiteLLM](/zh-CN/third-party-integrations#litellm)，这是一个开源工具，可帮助公司[按密钥跟踪支出](https://docs.litellm.ai/docs/proxy/virtual_keys#tracking-spend)。此项目与 Anthropic 无关，我们尚未审计其安全性。

### 速率限制建议

为团队设置 Claude Code 时，请根据您的组织规模考虑这些每用户的令牌每分钟 (TPM) 和请求每分钟 (RPM) 建议：

| 团队规模       | 每用户 TPM   | 每用户 RPM   |
| ---------- | --------- | --------- |
| 1-5 用户     | 200k-300k | 5-7       |
| 5-20 用户    | 100k-150k | 2.5-3.5   |
| 20-50 用户   | 50k-75k   | 1.25-1.75 |
| 50-100 用户  | 25k-35k   | 0.62-0.87 |
| 100-500 用户 | 15k-20k   | 0.37-0.47 |
| 500+ 用户    | 10k-15k   | 0.25-0.35 |

例如，如果您有 200 个用户，您可能会为每个用户请求 20k TPM，或总共 400 万 TPM (200\*20,000 = 400 万)。

随着团队规模的增长，每用户的 TPM 会减少，因为我们预计在较大的组织中，较少的用户会同时使用 Claude Code。这些速率限制适用于组织级别，而不是单个用户，这意味着当其他用户未积极使用该服务时，单个用户可以暂时消耗超过其计算份额的资源。

<Note>
  如果您预期会出现异常高的并发使用情况（例如与大型群体的实时培训会话），您可能需要更高的每用户 TPM 分配。
</Note>

## 减少令牌使用

* **压缩对话：**

  * 当上下文超过 95% 容量时，Claude 默认使用自动压缩
  * 切换自动压缩：运行 `/config` 并导航到"启用自动压缩"
  * 当上下文变大时手动使用 `/compact`
  * 添加自定义说明：`/compact Focus on code samples and API usage`
  * 通过添加到 CLAUDE.md 来自定义压缩：

    ```markdown  theme={null}
    # Summary instructions

    When you are using compact, please focus on test output and code changes
    ```

* **编写具体查询：** 避免触发不必要扫描的模糊请求

* **分解复杂任务：** 将大型任务分解为专注的交互

* **清除任务之间的历史记录：** 使用 `/clear` 重置上下文

成本可能会根据以下因素而有很大差异：

* 被分析的代码库的大小
* 查询的复杂性
* 被搜索或修改的文件数量
* 对话历史的长度
* 压缩对话的频率

## 后台令牌使用

Claude Code 即使在空闲时也会为某些后台功能使用令牌：

* **对话摘要：** 为 `claude --resume` 功能总结先前对话的后台作业
* **命令处理：** 某些命令（如 `/cost`）可能会生成请求以检查状态

这些后台进程即使没有主动交互，也会消耗少量令牌（通常每个会话不到 \$0.04）。

## 跟踪版本更改和更新

### 当前版本信息

要检查您当前的 Claude Code 版本和安装详情：

```bash  theme={null}
claude doctor
```

此命令显示您的版本、安装类型和系统信息。

### 了解 Claude Code 行为的变化

Claude Code 定期接收可能改变功能工作方式的更新，包括成本报告：

* **版本跟踪**：使用 `claude doctor` 查看您当前的版本
* **行为变化**：诸如 `/cost` 之类的功能可能在不同版本中显示不同的信息
* **文档访问**：Claude 始终可以访问最新文档，这可以帮助解释当前功能行为

### 当成本报告更改时

如果您注意到成本显示方式的变化（例如 `/cost` 命令显示不同的信息）：

1. **验证您的版本**：运行 `claude doctor` 以确认您当前的版本
2. **查阅文档**：直接询问 Claude 当前功能行为，因为它可以访问最新文档
3. **联系支持**：如有具体计费问题，请通过您的 Console 账户联系 Anthropic 支持

<Note>
  对于团队部署，我们建议从一个小型试点组开始，以在更广泛的推出之前建立使用模式。
</Note>
