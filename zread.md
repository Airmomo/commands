---
allowed-tools: mcp__zread__get_repo_structure, mcp__zread__read_file, mcp__zread__search_doc
argument-hint: [repo-name query or repo-name file-path]
description: Search and read GitHub repository documentation, code, and issues
---

# ZRead GitHub Repository Query

Query GitHub repositories to explore code structure, read files, and search documentation/issues.

## Usage

- `/zread [repo] [query]` - Search documentation and issues in a repository
- `/zread [repo] structure [path]` - Get directory structure
- `/zread [repo] read [file-path]` - Read a specific file

## Available Tools

### get_repo_structure
Get the directory structure and file list of a GitHub repository.
- `repo_name`: Repository in owner/repo format (e.g., "vitejs/vite")
- `dir_path`: Directory path (optional, defaults to root)

### read_file
Read the full content of a specific file in a repository.
- `repo_name`: Repository in owner/repo format
- `file_path`: Relative path to the file

### search_doc
Search documentation, issues, and commits in a repository.
- `repo_name`: Repository in owner/repo format
- `query`: Search keywords or question
- `language`: "zh" or "en"

## Implementation Steps

### 1. Parse Repository Information
Extract the repository name from user input:
- Format: owner/repo (e.g., facebook/react)
- Validate the format before proceeding

### 2. Determine Query Type
Based on user input, select the appropriate tool:
- Structure exploration → `get_repo_structure`
- File content needed → `read_file`
- Documentation/issues search → `search_doc`

### 3. Execute Query
Call the appropriate tool with the provided parameters.

### 4. Provide Answer
Based on the results:
- Summarize relevant findings
- Include code snippets if applicable
- Reference specific files or issues

### 5. Include Source Links
**Always include source links at the end of your response:**

Add a "Sources:" section with relevant links:
- GitHub file links: `https://github.com/[owner]/[repo]/blob/main/[file-path]`
- GitHub issue links: `https://github.com/[owner]/[repo]/issues/[number]`
- GitHub PR links: `https://github.com/[owner]/[repo]/pull/[number]`
- Repository root: `https://github.com/[owner]/[repo]`

Example:
```
## Sources:
- [vite/src/node/server.ts](https://github.com/vitejs/vite/blob/main/packages/vite/src/node/server.ts)
- [Issue #1234](https://github.com/vitejs/vite/issues/1234)
```

## Examples

- `/zread facebook/react hooks implementation`
- `/zread vitejs/vite structure packages`
- `/zread vuejs/core read packages/reactivity/src/ref.ts`
- `/zread vercel/next.js app router configuration`
