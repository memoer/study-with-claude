---
name: examples
description: Provides additional code examples for the current topic. Shows basic to advanced progression, edge cases, and real-world usage patterns. Use when user wants more code samples or different approaches.
---

# More Examples

## Purpose

Provide varied code examples to reinforce learning through multiple perspectives and use cases.

## When This Activates

- User says "more examples", "show me another example"
- `/examples [topic]`
- "Can you show different ways to do this?"
- "What about edge cases?"

## Example Categories to Cover

### 1. Basic Example
- Minimal, focused on core concept
- Heavy comments explaining each line

### 2. Practical Example
- Real-world use case
- Common patterns you'd see in production

### 3. Edge Cases
- Empty inputs, null values
- Boundary conditions
- Error scenarios

### 4. Anti-patterns
- What NOT to do
- Common mistakes with explanations

### 5. Alternative Approaches
- Different ways to solve same problem
- Trade-offs between approaches

## Response Format

```markdown
### 💻 More Examples: [Topic]

#### Basic Example
\`\`\`[language]
// Focused on core concept
\`\`\`

#### Real-World Example
\`\`\`[language]
// Practical application
\`\`\`

#### Edge Cases
\`\`\`[language]
// Handling special scenarios
\`\`\`

#### ❌ Common Mistake
\`\`\`[language]
// What not to do and why
\`\`\`

#### Alternative Approach
\`\`\`[language]
// Different way with trade-offs
\`\`\`
```

## Guidelines

- Progress from simple → complex
- Always include comments
- Show both working and broken code (labeled clearly)
- Connect examples to current lesson context
- Save examples to `./lessons/[topic-slug]/examples/` directory
