---
name: lessons
description: Manages lesson files - list all lessons, show specific lesson, export lessons to single file, or continue from a specific lesson number. Use for navigating and organizing saved lessons.
allowed-tools: Read, Glob, Write, Bash(ls:*), Bash(cat:*)
---

# Lessons Management

## Purpose

Navigate, view, and manage saved lesson files in the `./lessons/` directory.

## Commands

### List Lessons
- `/lessons` or `/lessons list`
- "List lessons", "Show all lessons"
- "What lessons do I have?"

### Show Specific Lesson
- `/lessons show 5` or `/lessons #5`
- "Show lesson #5"
- "Display lesson 15"

### Continue from Lesson
- `/lessons continue 10`
- "Continue from lesson #10"
- "Resume from lesson 5"

### Export All
- `/lessons export`
- "Export all lessons"
- "Combine lessons into one file"

## Response Formats

### List Lessons
```markdown
### 📚 Your Lessons

| # | Topic | Date | File |
|---|-------|------|------|
| 001 | [Topic] | YYYY-MM-DD | `lessons/topic/lesson-001.md` |
| 002 | [Topic] | YYYY-MM-DD | `lessons/topic/lesson-002.md` |

**Total:** X lessons across Y topics
```

### Show Lesson
```markdown
### 📖 Lesson #[N]: [Topic]

[Full lesson content]
```

### Continue from Lesson
```markdown
### ▶️ Resuming from Lesson #[N]

**Previous topic:** [Topic]
**Key concepts covered:** [Brief summary]

**Ready to continue!** What would you like to learn next?
- Continue with [related topic]?
- Review [previous concept]?
- Start something new?
```

### Export All
```markdown
### 📦 Exported Lessons

All lessons combined into: `./lessons/all-lessons-export.md`

**Contents:**
- X lessons
- Y topics
- Exported on: YYYY-MM-DD
```

## Process

### For List
1. Glob `./lessons/**/*.md`
2. Parse frontmatter for metadata
3. Sort by lesson number
4. Display as table

### For Show
1. Find lesson file matching number
2. Read full content
3. Display formatted

### For Continue
1. Find lesson #N
2. Read to understand context
3. Set next lesson number to N+1
4. Summarize what was covered
5. Suggest continuation

### For Export
1. Collect all lesson files
2. Sort by number
3. Combine with separators
4. Write to `./lessons/all-lessons-export.md`

## File Structure Expected
```
./lessons/
├── variables-python/
│   ├── lesson-001.md
│   └── lesson-002.md
├── async-javascript/
│   ├── lesson-003.md
│   └── examples/
└── all-lessons-export.md (generated)
```
