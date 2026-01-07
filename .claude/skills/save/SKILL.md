---
name: save
description: Manually saves the current lesson to the lessons directory. Use when auto-save was missed or user wants to explicitly save content.
allowed-tools: Write, Bash(mkdir:*)
---

# Save Lesson

## Purpose

Manually trigger saving the current lesson content to the `./lessons/` directory.

## When This Activates

- User says "save this", "save lesson"
- `/save`
- "Make sure this is saved"
- "Save to file"

## Process

1. Identify current topic from conversation
2. Determine correct lesson number (check existing files)
3. Create topic directory if needed: `./lessons/[topic-slug]/`
4. Format content with proper frontmatter
5. Write to `./lessons/[topic-slug]/lesson-[NNN].md`
6. Confirm save with file path

## File Format

```markdown
---
## 📚 Lesson #[NUMBER]
**Topic:** [Topic Name]
**Date:** [YYYY-MM-DD]
**Difficulty:** [Level]
**Tags:** #tag1 #tag2
---

[Lesson content...]
```

## Response Format

```markdown
### 💾 Lesson Saved

**File:** `./lessons/[topic-slug]/lesson-[NNN].md`
**Topic:** [Topic Name]
**Lesson #:** [Number]

✅ Successfully saved!
```

## Naming Convention

- **Directory:** lowercase, hyphenated topic name
  - "Variables in Python" → `variables-in-python`
  - "Async/Await" → `async-await`

- **File:** `lesson-[NNN].md` with zero-padded number
  - Lesson 1 → `lesson-001.md`
  - Lesson 15 → `lesson-015.md`

## Guidelines

- Check for existing lessons to avoid number conflicts
- Preserve lesson number sequence within topic
- Include all sections from the lesson response
- Create examples subdirectory if code examples exist
