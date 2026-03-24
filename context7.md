---
allowed-tools: mcp__context7__resolve-library-id, mcp__context7__query-docs
argument-hint: [your question about any library or framework]
description: Query up-to-date documentation for any programming library or framework using Context7
---

# Context7 Documentation Query

Query official documentation and code examples from Context7 for any programming library or framework before answering questions.

## Usage

- `/context7 [question]` - Query documentation related to your question

## Implementation Steps

### 1. Library Identification
Analyze the user's question to identify relevant libraries or frameworks:
- Extract library names (React, Vue, Express, TypeScript, etc.)
- Identify version requirements if specified
- Determine the specific topic or feature in question

### 2. Resolve Library ID
Use `mcp__context7__resolve-library-id` to get the Context7-compatible library ID:
- `libraryName`: The library name extracted from the question
- `query`: The user's original question for context

### 3. Query Documentation
Use `mcp__context7__query-docs` to fetch relevant documentation:
- `libraryId`: The ID obtained from step 2
- `query`: The specific question or topic to search for

### 4. Answer Based on Documentation
Provide accurate answers based on the retrieved documentation:
- Include code examples from the documentation
- Reference specific API methods or features
- Provide links to relevant sections if available

## Query Optimization

- Be specific in queries to get more accurate results
- Include version information if relevant
- Break down complex questions into focused queries

## Output Requirements

**Always include source links at the end of your response:**

After answering, add a "Sources:" section with relevant links:
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

## Examples

- `/context7 How to use useEffect in React?`
- `/context7 Express.js middleware handling`
- `/context7 TypeScript generics tutorial`
- `/context7 Next.js app router configuration`
