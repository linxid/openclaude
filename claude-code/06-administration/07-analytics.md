---
title: "分析"
description: "查看您组织的 Claude Code 部署的详细使用情况洞察和生产力指标。"
---

# 分析

> 查看您组织的 Claude Code 部署的详细使用情况洞察和生产力指标。

Claude Code 提供了一个分析仪表板，帮助组织了解开发者使用模式、跟踪生产力指标，并优化他们的 Claude Code 采用。

<Note>
  分析目前仅适用于通过 Claude Console 使用 Claude API 的 Claude Code 的组织。
</Note>

## 访问分析

导航到分析仪表板，地址为 [console.anthropic.com/claude-code](https://console.anthropic.com/claude-code)。

### 所需角色

* **主要所有者**
* **所有者**
* **账单**
* **管理员**
* **开发者**

<Note>
  具有**用户**、**Claude Code 用户**或**成员管理员**角色的用户无法访问分析。
</Note>

## 可用指标

### 接受的代码行数

Claude Code 编写的代码行总数，用户已在其会话中接受。

* 排除被拒绝的代码建议
* 不跟踪后续删除

### 建议接受率

用户接受代码编辑工具使用的次数百分比，包括：

* Edit
* Write
* NotebookEdit

### 活动

**users**：给定日期的活跃用户数（左 Y 轴上的数字）

**sessions**：给定日期的活跃会话数（右 Y 轴上的数字）

### 支出

**users**：给定日期的活跃用户数（左 Y 轴上的数字）

**spend**：给定日期的总支出（美元）（右 Y 轴上的数字）

### 团队洞察

**Members**：所有已向 Claude Code 进行身份验证的用户

* API 密钥用户按 **API 密钥标识符**显示
* OAuth 用户按**电子邮件地址**显示

**本月支出：** 当前月份的每用户总支出。

**本月代码行数：** 当前月份的每用户接受代码行总数。

## 有效使用分析

### 监控采用

跟踪团队成员状态以识别：

* 可以分享最佳实践的活跃用户
* 整个组织的整体采用趋势

### 衡量生产力

工具接受率和代码指标帮助您：

* 了解开发者对 Claude Code 建议的满意度
* 跟踪代码生成有效性
* 识别培训或流程改进的机会

## 相关资源

* [使用 OpenTelemetry 监控使用情况](/zh-CN/monitoring-usage)，用于自定义指标和警报
* [身份和访问管理](/zh-CN/iam)，用于角色配置
