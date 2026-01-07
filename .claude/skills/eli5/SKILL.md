---
name: eli5
description: Explain Like I'm 5. Simplifies complex programming concepts using everyday analogies, simple language, and relatable examples. Use when the current explanation is too complex or user wants a simpler breakdown.
---

# ELI5 - Explain Like I'm 5

## Purpose

Transform complex technical concepts into simple, accessible explanations using analogies a child could understand.

## When This Activates

- User says "ELI5", "explain simply", "I don't understand"
- `/eli5 [topic]`
- "Can you make that simpler?"
- "I'm confused about..."

## How to Explain

### 1. Use Real-World Analogies
- Variables → labeled boxes
- Functions → recipes
- APIs → restaurant waiters
- Databases → filing cabinets
- Loops → repetitive tasks like brushing teeth

### 2. Avoid Jargon
- Replace technical terms with everyday words
- If you must use a term, immediately explain it

### 3. Build Progressively
- Start with the simplest version
- Add complexity only if asked

### 4. Use Visual Metaphors
```
Think of a variable like a labeled jar in your kitchen:
- The label (variable name) tells you what's inside
- The contents (value) can change
- You always know where to find it
```

## Response Format

```markdown
### 🧒 ELI5: [Topic]

**Simple version:**
[1-2 sentence explanation a 5-year-old could understand]

**Analogy:**
[Real-world comparison]

**Example:**
[Simplest possible code example with comments]

**Want more detail?** Just ask and I'll explain the next level!
```

## Examples

**Complex:** "A closure is a function that captures variables from its lexical scope"

**ELI5:** "A closure is like a backpack. When a function is created, it packs up all the variables it can see around it. Even when the function travels somewhere else, it still has everything in its backpack!"
