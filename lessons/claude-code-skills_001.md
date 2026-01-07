---
## 📚 Lesson #001
**Topic:** Claude Code Skills - Introduction
**Date:** 2025-12-31
**Difficulty:** Beginner
**Tags:** #claude-code #skills #ai-agents #customization #automation

---

### 📌 Quick Summary

Claude Code Skills are reusable, file-based instructions that teach Claude how to perform specific tasks. They activate **automatically** when your request matches their description—no slash command needed.

---

### 🤔 Why It Matters

Without Skills, you'd repeat the same instructions every conversation:
- "Review this PR using our team's 10-point checklist..."
- "Write commit messages in our conventional format..."
- "Generate tests following our AAA pattern..."

With Skills, you write these instructions **once**, and Claude applies them automatically whenever relevant. This saves time and ensures consistency across your entire team.

---

### 📖 Detailed Explanation

#### The Big Picture: What Are Skills?

Think of Skills like **job training manuals** for Claude:

```
┌─────────────────────────────────────────────────────────┐
│                    Without Skills                        │
│                                                          │
│  You: "Review this code. Check for X, Y, Z..."          │
│  You: "Review this code. Check for X, Y, Z..." (again)  │
│  You: "Review this code. Check for X, Y, Z..." (again)  │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│                     With Skills                          │
│                                                          │
│  SKILL.md: "When reviewing code, always check X, Y, Z"  │
│                          ↓                               │
│  You: "Review this code"                                │
│  Claude: (automatically applies X, Y, Z checks)         │
└─────────────────────────────────────────────────────────┘
```

#### Skills vs Slash Commands

| Feature | Skills | Slash Commands |
|---------|--------|----------------|
| How to trigger | **Automatic** (Claude decides) | **Manual** (you type `/command`) |
| Best for | Recurring workflows | On-demand actions |
| Example | Code review standards | `/commit` to make a commit |

---

### 💻 Code Examples

#### Basic Example: Your First Skill

**Step 1: Create the directory**
```bash
mkdir -p ~/.claude/skills/my-reviewer
```

**Step 2: Create SKILL.md**
```yaml
---
name: my-reviewer
description: Reviews code for common issues. Use when reviewing code, PRs, or when user asks for code review.
---

# Code Review Checklist

When reviewing code, always check:

1. **Naming**: Are variables/functions clearly named?
2. **Single Responsibility**: Does each function do one thing?
3. **Error Handling**: Are errors properly caught?
4. **Tests**: Are there tests for new code?

## Output Format

Provide feedback as:
- ✅ Good: [what's done well]
- ⚠️ Suggestion: [improvements]
- ❌ Issue: [must fix]
```

**Step 3: Restart Claude Code and test**
```
You: "Can you review this function?"
Claude: (automatically loads your Skill and uses your checklist!)
```

#### Advanced Example: Multi-File Skill

For complex Skills, use **progressive disclosure**:

```
~/.claude/skills/api-builder/
├── SKILL.md          ← Main instructions (always loaded when triggered)
├── PATTERNS.md       ← Design patterns (loaded only when needed)
├── EXAMPLES.md       ← Code examples (loaded only when needed)
└── scripts/
    └── scaffold.py   ← Helper script
```

---

### ⚠️ Common Pitfalls

| Mistake | Problem | Fix |
|---------|---------|-----|
| Vague description | Skill never triggers | Include specific keywords: "Use when..." |
| Too much in SKILL.md | Wastes context tokens | Keep under 500 lines, use supporting files |
| Missing `---` markers | YAML parsing fails | Ensure exact frontmatter format |

---

### 🎯 Practice Challenge

**Create a "commit message helper" Skill:**

1. Create `~/.claude/skills/commit-helper/SKILL.md`
2. Write a description that triggers on commit-related requests
3. Include your preferred commit message format (conventional commits?)
4. Test by asking Claude to help write a commit message

---

### 🔗 Related Topics

- **Lesson #002**: Skill File Structure (YAML frontmatter deep dive)
- **Lesson #003**: Progressive Disclosure Pattern
- **Lesson #004**: Tool Restrictions with `allowed-tools`
- **Lesson #005**: Project vs Personal Skills

---

### ✅ Key Takeaways

1. **Skills = Reusable Instructions** - Write once, use automatically
2. **Model-Invoked** - Claude decides when to use them (not you)
3. **Description is Critical** - Include "Use when..." trigger keywords
4. **Start Simple** - One SKILL.md file is enough to begin

---
