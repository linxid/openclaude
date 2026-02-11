---
title: "开发容器"
description: "了解 Claude Code 开发容器，适用于需要一致、安全环境的团队。"
---

参考 [devcontainer 设置](https://github.com/anthropics/claude-code/tree/main/.devcontainer) 和相关的 [Dockerfile](https://github.com/anthropics/claude-code/blob/main/.devcontainer/Dockerfile) 提供了一个预配置的开发容器，你可以按原样使用，也可以根据需要进行自定义。此 devcontainer 与 Visual Studio Code [Dev Containers 扩展](https://code.visualstudio.com/docs/devcontainers/containers) 和类似工具兼容。

容器的增强安全措施（隔离和防火墙规则）允许你运行 `claude --dangerously-skip-permissions` 来绕过权限提示，以实现无人值守操作。

<Warning>
  虽然 devcontainer 提供了实质性的保护，但没有任何系统能够完全免疫所有攻击。
  当使用 `--dangerously-skip-permissions` 执行时，devcontainer 不会阻止恶意项目泄露 devcontainer 中可访问的任何内容，包括 Claude Code 凭证。
  我们建议仅在使用受信任的存储库进行开发时使用 devcontainer。
  始终保持良好的安全实践并监控 Claude 的活动。
</Warning>

## 主要功能

* **生产就绪的 Node.js**：基于 Node.js 20 构建，包含必要的开发依赖项
* **安全设计**：自定义防火墙限制网络访问仅限于必要的服务
* **开发者友好的工具**：包括 git、ZSH 及生产力增强功能、fzf 等
* **无缝 VS Code 集成**：预配置的扩展和优化的设置
* **会话持久性**：在容器重启之间保留命令历史和配置
* **随处可用**：兼容 macOS、Windows 和 Linux 开发环境

## 4 步快速开始

1. 安装 VS Code 和 Remote - Containers 扩展
2. 克隆 [Claude Code 参考实现](https://github.com/anthropics/claude-code/tree/main/.devcontainer) 存储库
3. 在 VS Code 中打开存储库
4. 出现提示时，点击"在容器中重新打开"（或使用命令面板：Cmd+Shift+P → "Remote-Containers: Reopen in Container"）

## 配置分解

devcontainer 设置由三个主要组件组成：

* [**devcontainer.json**](https://github.com/anthropics/claude-code/blob/main/.devcontainer/devcontainer.json)：控制容器设置、扩展和卷挂载
* [**Dockerfile**](https://github.com/anthropics/claude-code/blob/main/.devcontainer/Dockerfile)：定义容器镜像和已安装的工具
* [**init-firewall.sh**](https://github.com/anthropics/claude-code/blob/main/.devcontainer/init-firewall.sh)：建立网络安全规则

## 安全功能

容器通过其防火墙配置实现了多层安全方法：

* **精确访问控制**：将出站连接限制为仅白名单域（npm 注册表、GitHub、Claude API 等）
* **允许的出站连接**：防火墙允许出站 DNS 和 SSH 连接
* **默认拒绝策略**：阻止所有其他外部网络访问
* **启动验证**：在容器初始化时验证防火墙规则
* **隔离**：创建与主系统分离的安全开发环境

## 自定义选项

devcontainer 配置设计为可适应你的需求：

* 根据你的工作流添加或删除 VS Code 扩展
* 为不同的硬件环境修改资源分配
* 调整网络访问权限
* 自定义 shell 配置和开发者工具

## 示例用例

### 安全的客户端工作

使用 devcontainer 隔离不同的客户端项目，确保代码和凭证不会在环境之间混合。

### 团队入职

新团队成员可以在几分钟内获得完全配置的开发环境，所有必要的工具和设置都已预装。

### 一致的 CI/CD 环境

在 CI/CD 管道中镜像你的 devcontainer 配置，以确保开发和生产环境匹配。

## 相关资源

* [VS Code devcontainer 文档](https://code.visualstudio.com/docs/devcontainers/containers)
* [Claude Code 安全最佳实践](/zh-CN/security)
* [企业网络配置](/zh-CN/network-config)
