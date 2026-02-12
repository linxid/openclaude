# OpenClaude Chinese Community

English | [中文](./README.md)

OpenClaude is a community-driven documentation site providing high-quality Chinese documentation resources for AI development tools.

## Documentation

### Claude Code

[Claude Code](https://code.claude.com/) is Anthropic's official AI coding assistant. This site provides complete Chinese documentation including:

- **Getting Started** - Quick start, installation and setup
- **Core Concepts** - How it works, features overview, best practices
- **Platform Integration** - VS Code, JetBrains, Chrome extension, Slack
- **Advanced Features** - Skills, Hooks, MCP protocol, plugin system
- **Deployment** - GitHub Actions, GitLab CI/CD, headless mode
- **Enterprise Management** - Amazon Bedrock, Google Vertex AI, cost monitoring

### OpenClaw

[OpenClaw](https://openclaw.ai/) is an open-source AI Agent platform with multi-channel messaging integration. Documentation covers:

- **Core Concepts** - Agent architecture, session management, memory system
- **Chat Channels** - Telegram, WhatsApp, Discord, Slack, Feishu, etc.
- **Deployment** - Docker, Node.js, Nix, cloud platform deployment
- **Model Support** - Anthropic, OpenAI, Bedrock, Chinese models
- **CLI Commands** - Complete command-line tool reference

## Local Development

### Install Dependencies

```bash
npm i -g mint
```

### Start Development Server

```bash
mint dev
```

Visit `http://localhost:3000` in your browser to preview the documentation.

### Troubleshooting

- If the dev environment won't start, run `mint update` to update the CLI
- If pages show 404, make sure you're running in a directory with a valid `docs.json`

## Contributing

Pull requests are welcome to improve the documentation!

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -m 'Add some improvement'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Create a Pull Request

## Links

- [Claude Code Official Docs](https://code.claude.com/docs/zh-CN)
- [OpenClaw Official Docs](https://docs.openclaw.ai/zh-CN)
- [Mintlify Documentation](https://mintlify.com/docs)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
