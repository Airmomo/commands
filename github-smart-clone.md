---
description: 从 GitHub 智能克隆整个仓库或指定文件/文件夹，适用于只需获取部分代码或参考特定实现的场景，避免克隆整个大仓库。
argument-hint: <github-url> [--no-git]
allowed-tools: Bash
---

# GitHub 智能克隆命令

根据 URL 自动判断克隆类型：

- **完整仓库**：默认保留 git 历史，`--no-git` 可移除
- **部分文件/文件夹**：使用 sparse-checkout，不保留 git 历史

## 用户输入

URL: $ARGUMENTS

## 执行步骤

### 1. 解析 URL 判断克隆类型

分析用户提供的 GitHub URL，识别以下模式：

**完整仓库 URL 格式：**

- `https://github.com/owner/repo`
- `https://github.com/owner/repo.git`
- `git@github.com:owner/repo.git`

**部分路径 URL 格式：**

- `https://github.com/owner/repo/tree/main/path/to/folder`
- `https://github.com/owner/repo/blob/main/path/to/file.ext`

从 URL 中提取：

- `owner`: 仓库所有者
- `repo`: 仓库名称
- `branch`: 分支名（部分克隆时需要，默认 main）
- `path`: 文件或文件夹路径（部分克隆时）

### 2. 检查是否需要 --no-git 标志

检查 `$ARGUMENTS` 中是否包含 `--no-git` 参数：

- 如果包含，设置 `KEEP_GIT=false`
- 否则设置 `KEEP_GIT=true`（完整克隆默认保留）

### 3. 执行克隆操作

#### 情况 A：完整仓库克隆

```bash
# 克隆仓库
git clone https://github.com/{owner}/{repo}.git

# 如果用户指定了 --no-git，删除 .git 文件夹
if [ "$KEEP_GIT" = "false" ]; then
  rm -rf {repo}/.git
  echo "已删除 .git 文件夹，项目不再有 git 历史"
fi
```

#### 情况 B：部分克隆（文件/文件夹）

使用 sparse-checkout 进行高效部分克隆：

```bash
# 创建临时目录
TEMP_DIR="temp-{repo}-$(date +%s)"
TARGET_PATH="{path}"  # 从 URL 解析的路径

# 使用稀疏克隆
git clone --depth 1 --filter=blob:none --sparse https://github.com/{owner}/{repo}.git "$TEMP_DIR"
cd "$TEMP_DIR"

# 设置稀疏检出路径
git sparse-checkout set "$TARGET_PATH"

# 获取默认分支名（如果 URL 中未指定）
# 如果是 tree/blob URL，使用 URL 中的分支；否则尝试 main/master

# 移动目标到上级目录
# 如果是文件夹，移动整个文件夹
# 如果是单个文件，只移动该文件
if [ -d "$TARGET_PATH" ]; then
  mv "$TARGET_PATH" ../
else
  # 获取文件名
  FILENAME=$(basename "$TARGET_PATH")
  mv "$TARGET_PATH" "../$FILENAME"
fi

# 返回并清理临时目录
cd ..
rm -rf "$TEMP_DIR"

echo "部分克隆完成：$TARGET_PATH"
```

### 4. 输出结果

完成后输出：

- 克隆类型（完整/部分）
- 目标位置
- 是否保留 git 历史

## 使用示例

```bash
# 克隆完整仓库（保留 git 历史）
/github-smart-clone https://github.com/anthropics/claude-code

# 克隆完整仓库（不保留 git 历史）
/github-smart-clone https://github.com/anthropics/claude-code --no-git

# 克隆特定文件夹
/github-smart-clone https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills

# 克隆特定文件
/github-smart-clone https://github.com/anthropics/claude-code/blob/main/README.md
```

## 注意事项

1. **部分克隆不保留 git 历史**：只获取文件内容，无版本控制
2. **完整克隆默认保留 .git**：可用 `--no-git` 移除
3. 确保 git 已安装并可用
4. 目标路径不要与现有文件/文件夹冲突
