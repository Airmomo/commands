# Commands

斜杆命令（Slash Commands）是 Claude Code 中的一种可复用的提示模板，以 Markdown 文件形式存储，通过斜杠命令名`/`的方式快速调用。

本项目整理了我在日常开发中收集、创建和使用的命令，实用性和可靠性上都经过了验证。

## 安装命令

本项目下的所有命令原生支持在 [Claude Code](https://github.com/anthropics/claude-code) 中使用，直接安装：

```bash
git clone https://github.com/Airmomo/commands.git && cp -r commands/* ~/.claude/commands/ && rm -rf commands
```

## 命令列表

### 思考增强类

| 命令 | 简述 |
|------|------|
| [/think](./think.md) | 基础思考增强 |
| [/megathink](./megathink.md) | 深度思考，综合分析 |
| [/ultrathink](./ultrathink.md) | 最大化深度分析 |

---

### 版本控制类

| 命令 | 简述 |
|------|------|
| [/git-auto-commit](./git-auto-commit.md) | 分析变更并创建 Git 提交 |
| [/github-smart-clone](./github-smart-clone.md) | 智能克隆 Github 仓库，支持仅拉取文件/文件夹 |

---

### 文档与知识检索类

| 命令 | 简述 |
|------|------|
| [/context7-cli](./context7-cli.md) | 使用 Context7 CLI 查询热门代码库或框架的最新官方文档 |
| [/context7](./context7.md) | 使用 Context7 MCP 查询热门代码库或框架的最新官方文档 |
| [/zread](./zread.md) | 搜索和阅读 GitHub 仓库的文档、代码和问题 |
| [/openclaw-doc](./openclaw-doc.md) | 检索 OpenClaw 官方文档及其 GitHub 仓库代码后回答问题 |
| [/claude-code-doc](./claude-code-doc.md) | 检索 Claude Code 官方文档及其 GitHub 仓库代码后回答问题 |

---

### 开发调试类

| 命令 | 简述 |
|------|------|
| [/lint-command](./lint-command.md) | 检查斜杠命令文件是否符合开发规范，查漏补缺并修复 |
| [/parallel-debug](./parallel-debug.md) | 并行诊断调试，启动多个独立 Agent 从不同方向竞速排查错误根因并验证修复 |

---

### 按依赖类型汇总

| 类型 | 命令 |
|------|------|
| **开箱即用** | `/think` `/megathink` `/ultrathink` `/git-auto-commit` `/github-smart-clone` `/lint-command` |
| **需 CLI 支持** | `/context7-cli` |
| **需 MCP 支持** | `/context7` `/zread` `/openclaw-doc` `/claude-code-doc` `/parallel-debug` |

---

### MCP 服务安装参考

| MCP 服务 | 安装参考 |
|----------|----------|
| Context7 MCP | [Context7-Installation](https://github.com/upstash/context7#installation) |
| 开源仓库 MCP (ZRead) | [ZRead-Installation](https://docs.bigmodel.cn/cn/coding-plan/mcp/zread-mcp-server) |
| 联网搜索 MCP | [ZWeb-Search-Installation](https://docs.bigmodel.cn/cn/coding-plan/mcp/search-mcp-server) |

> [!NOTE]
> Context7 API 密钥可通过 [context7.com/dashboard](https://context7.com/dashboard) 免费获取，以提高请求频率限制。

## 如何创建一个优秀的命令

我总结了一个笔记[《Claude Code 斜杆命令开发指南》](docs/command-development.md)帮你快速了解如何创建一个标准且优秀的 Command。

**✨ 通过 Claude Code 技能创建（推荐）**

Claude Code 已内置支持了创建 Command 的 Skill ([Command Development for Claude Code](https://github.com/anthropics/claude-code/blob/main/plugins/plugin-dev/skills/command-development/SKILL.md))，当你发送`创建斜杠命令`，`添加命令`，`编写自定义命令`等提示时会帮你快速创建 Command。

## 使用命令的好处

1. 提高效率：不用每次重复输入相同的提示，一个斜杠 + 命令名即可完成复杂任务
2. 保证一致性：当团队成员使用相同的命令时，可确保输出格式、流程标准化，避免遗漏重要步骤
3. 高复用率：可将最佳实践固化成命令，方便快速复用
4. 易于管理；使用 Markdown 格式，简单直观，支持版本控制，跟踪变更，可按项目或个人组织命令
