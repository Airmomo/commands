---
description: 从 GitHub 智能克隆整个仓库或指定文件/文件夹，适用于只需获取部分代码或参考特定实现的场景，避免克隆整个大仓库。
argument-hint: <github-url> [--no-git]
allowed-tools: Bash
---

# GitHub 智能克隆命令

你收到用户输入：`$ARGUMENTS`

## 约束规则（必须严格遵守）

- **禁止探索**：不要使用 Glob、Grep、Read 等工具探索当前代码库，不要检查文件是否已存在
- **禁止创建额外文件**：只执行 git clone 相关操作，不创建任何辅助脚本、文档、报告或日志文件
- **禁止过度分析**：直接解析 URL 并执行 Bash 命令完成克隆，不要分多步验证
- **如果 URL 无法解析**：直接向用户确认，不要自行猜测或搜索

## 参数验证

如果 `$ARGUMENTS` 为空或未匹配到有效的 GitHub URL，直接告知用户正确用法并停止：

```
/github-smart-clone <github-url> [--no-git]
```

有效的 GitHub URL 格式：
- `https://github.com/owner/repo`
- `https://github.com/owner/repo.git`
- `git@github.com:owner/repo.git`
- `https://github.com/owner/repo/tree/{branch}/path`
- `https://github.com/owner/repo/blob/{branch}/path`

## URL 解析规则

从 `$ARGUMENTS` 中提取信息，识别两种模式：

| 模式 | URL 格式 | 提取内容 |
|------|----------|----------|
| 完整仓库 | `https://github.com/owner/repo` 或 `.git` 结尾 | owner, repo |
| 部分路径 | `.../tree/{branch}/path` 或 `.../blob/{branch}/path` | owner, repo, branch, path |

同时检查是否包含 `--no-git` 参数（仅对完整仓库有效）。

## 执行

解析 URL 后，通过 Bash 工具直接执行对应命令。以下为命令模板，将 `{owner}`、`{repo}`、`{path}` 替换为实际解析值：

### 完整仓库克隆

```bash
git clone https://github.com/{owner}/{repo}.git
```

若包含 `--no-git` 参数，克隆成功后追加执行：

```bash
rm -rf {repo}/.git
```

### 部分克隆（文件/文件夹）

```bash
# 创建临时目录，避免与已有文件冲突
TEMP="temp-{repo}-$(date +%s)"
# 浅克隆 + 稀疏检出，只下载目标路径
git clone --depth 1 --filter=blob:none --sparse https://github.com/{owner}/{repo}.git "$TEMP"
cd "$TEMP"
git sparse-checkout set "{path}"
# 文件夹直接移动，单文件提取文件名后移动
if [ -d "{path}" ]; then mv "{path}" ../; else mv "{path}" "../$(basename {path})"; fi
cd .. && rm -rf "$TEMP"
```

## 完成后

仅输出一行结果：克隆类型 + 目标位置 + 是否保留 git 历史。不做其他操作。
