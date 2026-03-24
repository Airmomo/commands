---
allowed-tools: mcp__context7__resolve-library-id, mcp__context7__query-docs, mcp__zread__get_repo_structure, mcp__zread__read_file, mcp__zread__search_doc
argument-hint: [your question about claude-code]
description: Query Claude Code documentation and GitHub repository code before answering questions
---

# Claude Code Documentation Query

**IMPORTANT: Always load context from Context7 and GitHub BEFORE answering the user's question.**

## Usage

- `/claude-code-doc [question]` - Query Claude Code documentation and code, then answer

## Context Loading Steps (MUST Execute First)

### Step 1: Query Context7 Documentation
Use Context7 tools to fetch Claude Code documentation:

1. Call `mcp__context7__resolve-library-id`
   - `libraryName`: "claude-code"
   - `query`: The user's question

2. Call `mcp__context7__query-docs`
   - `libraryId`: The ID from step 1
   - `query`: The user's question

### Step 2: Query GitHub Repository
Repository: `anthropics/claude-code`

Select appropriate ZRead tools based on the question type:

- **Structure exploration**: `mcp__zread__get_repo_structure`
  - `repo_name`: "anthropics/claude-code"
  - `dir_path`: Relevant directory based on question

- **Documentation/issues search**: `mcp__zread__search_doc`
  - `repo_name`: "anthropics/claude-code"
  - `query`: Keywords from user's question
  - `language`: "en"

- **File content**: `mcp__zread__read_file`
  - `repo_name`: "anthropics/claude-code"
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
| CLI commands | GitHub repo | read_file, search_doc |
| Configuration | docs/ directory | search_doc, read_file |
| Hooks & Skills | README + source | search_doc, read_file |
| Troubleshooting | Issues | search_doc |

## Common Topics

- CLI commands and options
- Hooks configuration (PreToolUse, PostToolUse, etc.)
- Skills and slash commands
- MCP server integration
- Settings and permissions
- Memory and context management

## Important Notes

- ⚠️ **Always load context BEFORE answering** - Do not rely on memory
- If Context7 lacks Claude Code docs, prioritize GitHub repository
- Check README, docs/, and CLAUDE.md files first
- For implementation details, read actual source files

## Output Requirements

**Always include source links at the end of your response:**

After answering, add a "Sources:" section with relevant links:
- Context7 documentation URLs (if available)
- GitHub file links: `https://github.com/anthropics/claude-code/blob/main/[file-path]`
- GitHub issue/PR links: `https://github.com/anthropics/claude-code/issues/[number]`
- Repository root: `https://github.com/anthropics/claude-code`

Example:
```
## Sources:
- [Claude Code README](https://github.com/anthropics/claude-code/blob/main/README.md)
- [Hooks Documentation](https://github.com/anthropics/claude-code/blob/main/docs/hooks.md)
```

## Examples

- `/claude-code-doc How to configure hooks?`
- `/claude-code-doc What is the settings.json format?`
- `/claude-code-doc How to create a custom skill?`
- `/claude-code-doc How to use MCP servers?`
- `/claude-code-doc Explain the memory system`
