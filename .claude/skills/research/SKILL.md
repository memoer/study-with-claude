---
name: research
description: Fetches latest documentation for programming libraries, frameworks, and tools. Use when studying current APIs, best practices, release notes, or feature updates. Triggers for requests like "What's new in React?", "Latest LangChain docs", "Current Docker best practices".
allowed-tools: WebSearch, WebFetch, Read, Bash(mcp-cli:*)
---

# Research Latest Documentation

## Purpose

Fetch and summarize current documentation to support learning with up-to-date information.

## When This Skill Activates

- "What's new in [library]?"
- "Latest docs for [framework]"
- "Current best practices for [tool]"
- "Check deprecations in [package]"
- `/research [topic]`

## Research Process

### Step 1: Identify Sources

Use **Context7 MCP** (preferred) for official library docs:
```bash
mcp-cli call plugin_context7_context7/resolve-library-id '{"libraryName": "[library]"}'
mcp-cli call plugin_context7_context7/query-docs '{"context7LibraryId": "[id]", "topic": "[topic]"}'
```

Use **WebSearch** for:
- Release notes and changelogs
- Blog posts and tutorials
- Comparison articles

### Step 2: Fetch Content

- Official documentation pages
- GitHub READMEs and release pages
- Authoritative blog posts

### Step 3: Format Output

Always include in response:

```markdown
### 🔍 Research Sources
**Last Updated:** [YYYY-MM-DD]
**Documentation Version:** [version if applicable]

**Sources:**
- [Source Title](URL) - Brief description

**Note:** Information accurate as of [date]. Check official docs for latest.
```

## Tool Priority

| Priority | Tool | Best For |
|----------|------|----------|
| 1st | Context7 MCP | Official library docs, code examples |
| 2nd | WebSearch | Trends, comparisons, release notes |
| 3rd | WebFetch | Specific doc pages, GitHub READMEs |

## Example Usage

**User:** "What's new in React 19?"

**Process:**
1. Query Context7 for React documentation
2. WebSearch for "React 19 release notes 2025"
3. Summarize key features with version info
4. Include source links

## Popular Documentation Sources

| Library | Primary Source |
|---------|----------------|
| React | react.dev |
| TypeScript | typescriptlang.org |
| Python | python.org/docs |
| LangChain | python.langchain.com |
| FastAPI | fastapi.tiangolo.com |
| Docker | docs.docker.com |
| Node.js | nodejs.org/docs |
