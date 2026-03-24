---
allowed-tools: Bash(ctx7:*), Bash(npx:ctx7:*)
argument-hint: [your question about any library or framework]
description: Query up-to-date documentation for any programming library or framework using Context7 CLI
---

# Context7 Documentation Query

Query official documentation and code examples from Context7 for any programming library or framework before answering questions.

## Usage

- `/context7-cli [question]` - Query documentation related to your question

## Prerequisites

Ensure Context7 CLI is available:

```bash
# Install or update CLI
npm install -g ctx7@latest

# Or run directly without installing
npx ctx7@latest <command>
```

## Implementation Workflow

Two-step process: resolve library name to ID, then query docs with that ID.

### Step 1: Resolve Library ID

Resolve the library name to a Context7-compatible ID:

```bash
ctx7 library <name> <query>
```

**Parameters:**

- `<name>`: Library name (React, Next.js, Prisma, etc.)
- `<query>`: User's original question (required for ranking)

**Examples:**

```bash
ctx7 library react "How to clean up useEffect with async operations"
ctx7 library nextjs "How to set up app router with middleware"
ctx7 library prisma "How to define one-to-many relations with cascade delete"
```

**Result Analysis:**
Each result includes:

- **Library ID**: Context7 identifier (format: `/org/project`)
- **Name**: Library or package name
- **Description**: Short summary
- **Code Snippets**: Number of available code examples
- **Source Reputation**: Authority indicator (High, Medium, Low)
- **Benchmark Score**: Quality indicator (100 is highest)
- **Versions**: Available versions if any

**Selection Process:**

1. Analyze query to identify the library
2. Select based on:
   - Name similarity (exact matches prioritized)
   - Description relevance
   - Documentation coverage (higher snippet counts)
   - Source reputation (High/Medium preferred)
   - Benchmark score (higher is better)
3. For version-specific queries, use version-specific ID: `/org/project/version`

### Step 2: Query Documentation

Fetch documentation using the resolved library ID:

```bash
ctx7 docs <libraryId> <query>
```

**Examples:**

```bash
ctx7 docs /facebook/react "How to clean up useEffect with async operations"
ctx7 docs /vercel/next.js "How to add authentication middleware to app router"
ctx7 docs /prisma/prisma "How to define one-to-many relations with cascade delete"
```

**Query Best Practices:**

- Be specific and include relevant details
- Use the user's full question when possible
- Avoid vague one-word queries
- Do not include sensitive information (API keys, passwords, credentials)

| Quality | Example                                                    |
| ------- | ---------------------------------------------------------- |
| Good    | `"How to set up authentication with JWT in Express.js"`    |
| Good    | `"React useEffect cleanup function with async operations"` |
| Bad     | `"auth"`                                                   |
| Bad     | `"hooks"`                                                  |

## Important Constraints

- Library IDs require `/` prefix: `/facebook/react` not `facebook/react`
- Always run `ctx7 library` first
- Do not run more than 3 times per question
- If unable to find after 3 attempts, use best available result

## Error Handling

### Quota Exceeded

If command fails with quota error ("Monthly quota reached" or "quota exceeded"):

1. **Inform user**: Context7 quota is exhausted
2. **Suggest authentication**:

   ```bash
   # Option A: Environment variable
   export CONTEXT7_API_KEY=your_key

   # Option B: OAuth login
   ctx7 login
   ```

3. **Fallback**: Answer from training knowledge with clear note about potential outdated information

**Do not silently fall back** — always inform user why Context7 was not used.

### Library Not Found

If no good matches exist:

1. Clearly state this
2. Suggest query refinements
3. Request clarification for ambiguous queries

## Implementation Steps

### 1. Library Identification

Analyze the user's question to identify:

- Library names (React, Vue, Express, TypeScript, etc.)
- Version requirements if specified
- Specific topic or feature in question

### 2. Resolve Library ID

```bash
ctx7 library <name> "$ARGUMENTS"
```

Select the most appropriate library ID from results.

### 3. Query Documentation

```bash
ctx7 docs <libraryId> "$ARGUMENTS"
```

### 4. Answer Based on Documentation

Provide accurate answers based on retrieved documentation:

- Include code examples from documentation
- Reference specific API methods or features
- Provide links to relevant sections

## Output Requirements

**Always include source links at the end of your response:**

Add a "Sources:" section with:

- Context7 documentation URLs
- Official library documentation links
- GitHub repository links (if applicable)
- API reference links

Example:

```
## Sources:
- [React Official Docs - useEffect](https://react.dev/reference/react/useEffect)
- [Context7 React Documentation](https://context7.com/library/react)
```

## Common Mistakes to Avoid

- ❌ Missing `/` prefix: `facebook/react` → ✅ `/facebook/react`
- ❌ Skipping `ctx7 library`: `ctx7 docs react "hooks"` → ✅ Resolve ID first
- ❌ Vague queries: `"hooks"` → ✅ `"React useEffect cleanup function"`
- ❌ Sensitive data in queries: Include API keys, passwords
- ❌ Excessive queries: More than 3 per question

## Examples

- `/context7-cli How to use useEffect in React?`
- `/context7-cli Express.js middleware handling`
- `/context7-cli TypeScript generics tutorial`
- `/context7-cli Next.js app router configuration`
- `/context7-cli Prisma one-to-many relations with cascade delete`
