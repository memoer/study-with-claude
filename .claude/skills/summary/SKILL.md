---
name: summary
description: Provides a recap of recent lessons and key concepts learned. Summarizes main points, connections between topics, and suggests next steps. Use for review sessions or session wrap-up.
allowed-tools: Read, Glob
---

# Summary / Recap

## Purpose

Consolidate learning by summarizing recent lessons, highlighting connections, and reinforcing key takeaways.

## When This Activates

- User says "summary", "recap", "what did we cover?"
- `/summary`
- "Review today's lessons"
- End of study session

## What to Include

### 1. Lessons Covered
- List recent lesson numbers and topics
- Brief description of each

### 2. Key Concepts
- Main ideas from each lesson
- Important terminology defined

### 3. Connections
- How topics relate to each other
- Building blocks identified

### 4. Code Highlights
- Most important code patterns
- Snippets worth remembering

### 5. Next Steps
- Suggested topics to learn next
- Practice recommendations

## Response Format

```markdown
### 📋 Session Summary

#### Lessons Covered
| # | Topic | Key Concept |
|---|-------|-------------|
| 001 | [Topic] | [Main idea] |
| 002 | [Topic] | [Main idea] |

#### 🔑 Key Takeaways
1. [Important concept 1]
2. [Important concept 2]
3. [Important concept 3]

#### 🔗 How They Connect
[Explanation of relationships between topics]

#### 💻 Code to Remember
\`\`\`[language]
// Most important pattern from session
\`\`\`

#### ➡️ Suggested Next Steps
- [ ] Practice: [specific exercise]
- [ ] Read: [related topic]
- [ ] Build: [mini-project idea]

#### 📁 Your Lessons
Saved to: `./lessons/[topic-slug]/`
```

## Process

1. Read recent lesson files from `./lessons/` directory
2. Identify current topic context from conversation
3. Synthesize key points across lessons
4. Highlight practical applications
5. Suggest logical next topics

## Guidelines

- Keep summary concise but complete
- Focus on "aha moments" and key insights
- Reinforce correct understanding
- Gently correct any misconceptions observed
- Make next steps actionable and specific
