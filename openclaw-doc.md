---
allowed-tools: mcp__context7__resolve-library-id, mcp__context7__query-docs, mcp__zread__get_repo_structure, mcp__zread__read_file, mcp__zread__search_doc
argument-hint: [your question about openclaw]
description: Query openclaw documentation and GitHub repository code before answering questions
---

# OpenClaw Documentation Query

**IMPORTANT: Always load context from Context7 and GitHub BEFORE answering the user's question.**

## Usage

- `/openclaw-doc [question]` - Query openclaw documentation and code, then answer

## Context Loading Steps (MUST Execute First)

### Step 1: Query Context7 Documentation
Use Context7 tools to fetch openclaw documentation:

1. Call `mcp__context7__resolve-library-id`
   - `libraryName`: "openclaw"
   - `query`: The user's question

2. Call `mcp__context7__query-docs`
   - `libraryId`: The ID from step 1
   - `query`: The user's question

### Step 2: Query GitHub Repository
Repository: `openclaw/openclaw`

Select appropriate ZRead tools based on the question type:

- **Structure exploration**: `mcp__zread__get_repo_structure`
  - `repo_name`: "openclaw/openclaw"
  - `dir_path`: Relevant directory based on question

- **Documentation/issues search**: `mcp__zread__search_doc`
  - `repo_name`: "openclaw/openclaw"
  - `query`: Keywords from user's question
  - `language`: "zh"

- **File content**: `mcp__zread__read_file`
  - `repo_name`: "openclaw/openclaw"
  - `file_path`: Relevant file path

### Step 3: Answer Based on Context
After gathering context:
1. Synthesize information from both sources
2. Provide accurate, detailed answers
3. Include code examples and references

## Query Strategy

| Question Type | Primary Source | Tools |
|---------------|----------------|-------|
| How to use | Context7 + README | query-docs, search_doc |
| Code implementation | GitHub repo | read_file, get_repo_structure |
| Configuration | docs/ directory | search_doc, read_file |
| Troubleshooting | Issues | search_doc |

## Important Notes

- ⚠️ **Always load context BEFORE answering** - Do not rely on memory
- If Context7 lacks openclaw docs, prioritize GitHub repository
- Check README, docs/, and source code directories first
- For code questions, read actual source files for accuracy

## Output Requirements

**Always include source links at the end of your response:**

After answering, add a "Sources:" section with relevant links:
- Context7 documentation URLs (if available)
- GitHub file links: `https://github.com/openclaw/openclaw/blob/main/[file-path]`
- GitHub issue/PR links: `https://github.com/openclaw/openclaw/issues/[number]`
- Repository root: `https://github.com/openclaw/openclaw`

Example:
```
## Sources:
- [OpenClaw README](https://github.com/openclaw/openclaw/blob/main/README.md)
- [Configuration Docs](https://github.com/openclaw/openclaw/blob/main/docs/configuration.md)
```

## Examples

- `/openclaw-doc How to create an agent?`
- `/openclaw-doc What is the configuration file format?`
- `/openclaw-doc How to use MCP servers?`
- `/openclaw-doc Explain the pairing mechanism`
