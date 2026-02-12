# OpenClaude 中文社区

[English](./README_EN.md) | 中文

OpenClaude 是一个社区驱动的中文文档站点，为中文用户提供 AI 开发工具的高质量文档资源。

## 文档内容

### Claude Code

[Claude Code](https://code.claude.com/) 是 Anthropic 官方的 AI 编程助手，本站提供完整的中文文档，包括：

- **入门指南** - 快速开始、安装设置
- **核心概念** - 工作原理、功能概览、最佳实践
- **平台集成** - VS Code、JetBrains、Chrome 扩展、Slack
- **高级功能** - Skills、Hooks、MCP 协议、插件系统
- **部署运维** - GitHub Actions、GitLab CI/CD、无头模式
- **企业管理** - Amazon Bedrock、Google Vertex AI、成本监控

### OpenClaw

[OpenClaw](https://openclaw.ai/) 是一个开源的 AI Agent 平台，支持多渠道消息集成。文档涵盖：

- **核心概念** - Agent 架构、会话管理、记忆系统
- **聊天渠道** - Telegram、WhatsApp、Discord、Slack、飞书等
- **部署方式** - Docker、Node.js、Nix、云平台部署
- **模型支持** - Anthropic、OpenAI、Bedrock、国内模型
- **CLI 命令** - 完整的命令行工具参考

## 本地开发

### 安装依赖

```bash
npm i -g mint
```

### 启动开发服务器

```bash
mint dev
```

在浏览器访问 `http://localhost:3000` 预览文档。

### 常见问题

- 如果开发环境无法启动，运行 `mint update` 更新 CLI
- 如果页面 404，确保在包含 `docs.json` 的目录下运行

## 贡献指南

欢迎提交 Pull Request 改进文档！

1. Fork 本仓库
2. 创建功能分支 (`git checkout -b feature/improvement`)
3. 提交更改 (`git commit -m 'Add some improvement'`)
4. 推送到分支 (`git push origin feature/improvement`)
5. 创建 Pull Request

## 相关链接

- [Claude Code 官方文档](https://code.claude.com/docs/zh-CN)
- [OpenClaw 官方文档](https://docs.openclaw.ai/zh-CN)
- [Mintlify 文档](https://mintlify.com/docs)

## 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件。
