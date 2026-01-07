---
name: deeper
description: Provides deeper technical details about the current topic. Covers internals, implementation details, performance implications, and advanced concepts. Use when user wants to go beyond basics.
---

# Go Deeper

## Purpose

Dive into technical details, internals, and advanced concepts for users ready to understand the "how" and "why" behind the scenes.

## When This Activates

- User says "go deeper", "more details", "how does it work internally?"
- `/deeper [topic]`
- "What's happening under the hood?"
- "Why is it designed this way?"

## Topics to Cover

### 1. Implementation Details
- How it works internally
- Data structures used
- Algorithm complexity

### 2. Performance Implications
- Time/space complexity
- Memory usage
- Optimization opportunities

### 3. Design Decisions
- Why was it built this way?
- Historical context
- Trade-offs made

### 4. Edge Cases & Gotchas
- Boundary conditions
- Common pitfalls
- Platform-specific behaviors

### 5. Related Internals
- Connected systems
- Dependencies
- How pieces fit together

## Response Format

```markdown
### 🔬 Deep Dive: [Topic]

#### Under the Hood
[How it actually works internally]

#### Performance Characteristics
| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| ... | O(?) | O(?) | ... |

#### Design Rationale
[Why it's designed this way, historical context]

#### Advanced Gotchas
- [Non-obvious behavior 1]
- [Non-obvious behavior 2]

#### Source Code Reference
[Where to look in source for more understanding]

#### Further Reading
- [Resource 1]
- [Resource 2]
```

## Guidelines

- Assume solid foundation from basic lesson
- Use precise technical terminology (with definitions)
- Include complexity analysis where relevant
- Reference authoritative sources (specs, source code, docs)
- Connect to real-world implications
