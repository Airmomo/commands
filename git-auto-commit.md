---
allowed-tools: Bash, Read, Grep, Glob
argument-hint: [file or folder or none]
description: Automatically analyze changes and create git commits with intelligent messages
---

# Git Auto Commit Command

Automatically analyze changes using `git diff` and create commits with intelligent commit messages based on:
- Project development guidelines and policies
- Recent conversation context
- File change analysis and relationships
- Development progress and patterns

## Usage

- `/git-auto-commit` - Analyze all unstaged changes in the project
- `/git-auto-commit [file]` - Commit changes to a specific file
- `/git-auto-commit [folder]` - Commit changes to a specific folder

## Implementation Steps

### 1. Context Analysis
First, understand the project context:
- Current git status: `git status`
- Recent commit history: `git log --oneline -5`
- Project guidelines: @CLAUDE.md (if exists)
- README for project understanding: @README.md (if exists)

### 2. Change Analysis
Analyze the changes to be committed:
`git diff --name-only`
`git diff --stat`

### 3. Intelligent Grouping
Group changes logically based on:
- File types and modules
- Functional relationships
- Dependency patterns
- Feature coherence

### 4. Commit Message Generation
For each group of changes, generate commit messages that:
- Follow the project's existing commit message style
- Explain the purpose and impact of changes
- Include relevant technical details
- Use appropriate prefixes (feat:, fix:, refactor:, docs:, etc.)

### 5. Execution
Execute the commits:
`git add $TARGET_FILES`
`git commit -m "$GENERATED_MESSAGE"`

## Smart Message Generation Logic

The command analyzes:
1. **File patterns**: Configuration files, source code, documentation
2. **Change types**: New features, bug fixes, refactoring, documentation
3. **Project patterns**: Existing commit message style and conventions
4. **Content analysis**: What the changes actually do and why

## Examples

If no arguments are provided, it will analyze all uncommitted files and create one or more logical commits based on the relationships between changes.

If a file or folder is specified, it will focus only on changes within that scope.