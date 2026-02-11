---
title: "管理 Claude 的内存"
description: "了解如何通过不同的内存位置和最佳实践在会话中管理 Claude Code 的内存。"
---

Claude Code 可以在会话中记住您的偏好设置，例如样式指南和工作流中的常用命令。

## 确定内存类型

Claude Code 在分层结构中提供四个内存位置，每个位置都有不同的用途：

| 内存类型         | 位置                                                                                                                                                              | 用途                      | 用例示例                 | 共享对象         |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | -------------------- | ------------ |
| **企业策略**     | • macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`<br />• Linux: `/etc/claude-code/CLAUDE.md`<br />• Windows: `C:\Program Files\ClaudeCode\CLAUDE.md` | 由 IT/DevOps 管理的组织范围内的说明 | 公司编码标准、安全策略、合规要求     | 组织中的所有用户     |
| **项目内存**     | `./CLAUDE.md` 或 `./.claude/CLAUDE.md`                                                                                                                           | 项目的团队共享说明               | 项目架构、编码标准、常见工作流      | 通过源代码控制的团队成员 |
| **项目规则**     | `./.claude/rules/*.md`                                                                                                                                          | 模块化、特定主题的项目说明           | 特定于语言的指南、测试约定、API 标准 | 通过源代码控制的团队成员 |
| **用户内存**     | `~/.claude/CLAUDE.md`                                                                                                                                           | 所有项目的个人偏好设置             | 代码样式偏好、个人工具快捷方式      | 仅您（所有项目）     |
| **项目内存（本地）** | `./CLAUDE.local.md`                                                                                                                                             | 个人的项目特定偏好设置             | 您的沙箱 URL、首选测试数据      | 仅您（当前项目）     |

启动 Claude Code 时，所有内存文件都会自动加载到其上下文中。层次结构中较高位置的文件优先级更高，并首先加载，为更具体的内存提供基础。

<Note>
  CLAUDE.local.md 文件会自动添加到 .gitignore，使其成为不应签入版本控制的私有项目特定偏好设置的理想选择。
</Note>

## CLAUDE.md 导入

CLAUDE.md 文件可以使用 `@path/to/import` 语法导入其他文件。以下示例导入 3 个文件：

```
See @README for project overview and @package.json for available npm commands for this project.

# Additional Instructions
- git workflow @docs/git-instructions.md
```

允许相对路径和绝对路径。特别是，导入用户主目录中的文件是团队成员提供不签入存储库的个人说明的便捷方式。导入是 CLAUDE.local.md 的替代方案，在多个 git 工作树中效果更好。

```
# Individual Preferences
- @~/.claude/my-project-instructions.md
```

为了避免潜在的冲突，导入不会在 markdown 代码跨度和代码块内进行评估。

```
This code span will not be treated as an import: `@anthropic-ai/claude-code`
```

导入的文件可以递归导入其他文件，最大深度为 5 跳。您可以通过运行 `/memory` 命令查看加载了哪些内存文件。

## Claude 如何查找内存

Claude Code 递归读取内存：从当前工作目录开始，Claude Code 向上递归到（但不包括）根目录 */* 并读取它找到的任何 CLAUDE.md 或 CLAUDE.local.md 文件。当在大型存储库中工作时，这特别方便，您在 *foo/bar/* 中运行 Claude Code，并在 *foo/CLAUDE.md* 和 *foo/bar/CLAUDE.md* 中都有内存。

Claude 还会发现当前工作目录下的子树中嵌套的 CLAUDE.md。它们不是在启动时加载，而是仅在 Claude 读取这些子树中的文件时才包含。

## 使用 `/memory` 直接编辑内存

在会话期间使用 `/memory` 斜杠命令在系统编辑器中打开任何内存文件，以进行更广泛的添加或组织。

## 设置项目内存

假设您想设置一个 CLAUDE.md 文件来存储重要的项目信息、约定和常用命令。项目内存可以存储在 `./CLAUDE.md` 或 `./.claude/CLAUDE.md` 中。

使用以下命令为您的代码库引导 CLAUDE.md：

```
> /init
```

<Tip>
  提示：

  * 包含常用命令（构建、测试、lint）以避免重复搜索
  * 记录代码样式偏好和命名约定
  * 添加特定于您的项目的重要架构模式
  * CLAUDE.md 内存可用于与您的团队共享的说明和您的个人偏好。
</Tip>

## 使用 `.claude/rules/` 的模块化规则

对于较大的项目，您可以使用 `.claude/rules/` 目录将说明组织到多个文件中。这允许团队维护专注、组织良好的规则文件，而不是一个大型 CLAUDE.md。

### 基本结构

在项目的 `.claude/rules/` 目录中放置 markdown 文件：

```
your-project/
├── .claude/
│   ├── CLAUDE.md           # Main project instructions
│   └── rules/
│       ├── code-style.md   # Code style guidelines
│       ├── testing.md      # Testing conventions
│       └── security.md     # Security requirements
```

`.claude/rules/` 中的所有 `.md` 文件都会自动加载为项目内存，优先级与 `.claude/CLAUDE.md` 相同。

### 特定于路径的规则

规则可以使用带有 `paths` 字段的 YAML 前置事项限定到特定文件。这些条件规则仅在 Claude 处理与指定模式匹配的文件时适用。

```markdown  theme={null}
---
paths: src/api/**/*.ts
---

# API Development Rules

- All API endpoints must include input validation
- Use the standard error response format
- Include OpenAPI documentation comments
```

没有 `paths` 字段的规则会无条件加载，适用于所有文件。

### Glob 模式

`paths` 字段支持标准 glob 模式：

| 模式                     | 匹配                     |
| ---------------------- | ---------------------- |
| `**/*.ts`              | 任何目录中的所有 TypeScript 文件 |
| `src/**/*`             | `src/` 目录下的所有文件        |
| `*.md`                 | 项目根目录中的 Markdown 文件    |
| `src/components/*.tsx` | 特定目录中的 React 组件        |

您可以使用大括号有效地匹配多个模式：

```markdown  theme={null}
---
paths: src/**/*.{ts,tsx}
---

# TypeScript/React Rules
```

这会扩展为匹配 `src/**/*.ts` 和 `src/**/*.tsx`。您也可以用逗号组合多个模式：

```markdown  theme={null}
---
paths: {src,lib}/**/*.ts, tests/**/*.test.ts
---
```

### 子目录

规则可以组织到子目录中以获得更好的结构：

```
.claude/rules/
├── frontend/
│   ├── react.md
│   └── styles.md
├── backend/
│   ├── api.md
│   └── database.md
└── general.md
```

所有 `.md` 文件都会被递归发现。

### 符号链接

`.claude/rules/` 目录支持符号链接，允许您在多个项目中共享常见规则：

```bash  theme={null}
# Symlink a shared rules directory
ln -s ~/shared-claude-rules .claude/rules/shared

# Symlink individual rule files
ln -s ~/company-standards/security.md .claude/rules/security.md
```

符号链接会被解析，其内容会正常加载。循环符号链接会被检测并妥善处理。

### 用户级规则

您可以在 `~/.claude/rules/` 中创建适用于所有项目的个人规则：

```
~/.claude/rules/
├── preferences.md    # Your personal coding preferences
└── workflows.md      # Your preferred workflows
```

用户级规则在项目规则之前加载，使项目规则具有更高的优先级。

<Tip>
  `.claude/rules/` 的最佳实践：

  * **保持规则专注**：每个文件应涵盖一个主题（例如 `testing.md`、`api-design.md`）
  * **使用描述性文件名**：文件名应指示规则涵盖的内容
  * **谨慎使用条件规则**：仅在规则确实适用于特定文件类型时添加 `paths` 前置事项
  * **使用子目录组织**：分组相关规则（例如 `frontend/`、`backend/`）
</Tip>

## 组织级内存管理

组织可以部署适用于所有用户的集中管理的 CLAUDE.md 文件。

要设置组织级内存管理：

1. 在上面 [内存类型表](#determine-memory-type) 中显示的**托管策略**位置创建托管内存文件。

2. 通过您的配置管理系统（MDM、组策略、Ansible 等）部署，以确保在所有开发人员计算机上一致分发。

## 内存最佳实践

* **具体明确**："使用 2 空格缩进"比"正确格式化代码"更好。
* **使用结构来组织**：将每个单独的内存格式化为项目符号，并在描述性 markdown 标题下分组相关内存。
* **定期审查**：随着项目的发展更新内存，以确保 Claude 始终使用最新的信息和上下文。
