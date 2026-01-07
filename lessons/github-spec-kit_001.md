---
## 📚 Lesson #001
**Topic:** GitHub Spec Kit & Spec-Driven Development
**Date:** 2025-12-24
**Difficulty:** Intermediate
**Tags:** #ai #development-workflow #github #specifications #sdd
---

### 📌 Quick Summary
Spec Kit is GitHub's open-source toolkit for **Spec-Driven Development (SDD)** — a methodology where detailed specifications become the single source of truth that guides AI agents to generate working software reliably.

### 🤔 Why It Matters
As AI coding assistants become more powerful, the challenge shifts from "can AI write code?" to "how do we ensure AI writes the *right* code?" Spec Kit addresses this by creating a structured workflow where you define **what** you want before AI figures out **how** to build it.

### 📖 Detailed Explanation

**What is Spec-Driven Development?**

Think of it like this: Instead of giving a contractor vague instructions ("build me a nice house"), you provide detailed blueprints, material specifications, and non-negotiable requirements *before* construction begins.

**Key Components:**

1. **`constitution.md`** - Defines non-negotiable project rules
   - Required testing standards
   - Framework version requirements
   - Coding style guidelines

2. **Specification Files** - Detailed descriptions of what you're building
   - Features and requirements
   - Expected behaviors
   - Constraints and edge cases

3. **Slash Commands** - Workflow management
   - `/specify` - Define requirements
   - `/plan` - Create implementation plan
   - `/task` - Break down into actionable tasks

**The Workflow:**
```
Define Specs → Plan → Break into Tasks → AI Implements → Validate Against Specs
```

### 💻 Code Examples

#### Basic Example: constitution.md
```markdown
# Project Constitution

## Non-Negotiables
- All code must have unit tests with >80% coverage
- Use TypeScript strict mode
- No console.log in production code
- All API endpoints must be documented

## Tech Stack
- Node.js 20+
- React 18+
- PostgreSQL for database
```

#### Advanced Example: Feature Specification
```markdown
# Feature: User Authentication

## Requirements
- Users can register with email/password
- Passwords must be hashed using bcrypt
- JWT tokens expire after 24 hours
- Rate limit: 5 failed attempts = 15 min lockout

## Acceptance Criteria
- [ ] Registration creates user in database
- [ ] Login returns valid JWT
- [ ] Invalid credentials return 401
- [ ] Locked accounts return 429
```

### ⚠️ Common Pitfalls
- **Too vague specs** - "Build a good login" vs "Build login with OAuth2 support for Google/GitHub"
- **Skipping the planning phase** - Jumping straight to `/task` without proper `/plan`
- **Ignoring constitution.md** - Not enforcing your non-negotiables defeats the purpose
- **Over-specifying tech** early - Focus on *what*, let AI help decide *how*

### 🎯 Practice Challenge
Try creating a `constitution.md` for a simple todo app project:
1. Define 3 non-negotiable rules
2. Specify your tech stack constraints
3. Write a feature spec for "Add Todo Item"

### 🔗 Related Topics
- [GitHub Spec Kit Repository](https://github.com/github/spec-kit)
- [Microsoft Dev Blog on SDD](https://developer.microsoft.com/blog/spec-driven-development-spec-kit)
- [LogRocket Tutorial](https://blog.logrocket.com/github-spec-kit/)
- Requirements Engineering
- Test-Driven Development (TDD)
- AI-Assisted Coding with Copilot/Claude

### ✅ Key Takeaways
- Spec Kit provides structure for AI-assisted development
- **Specifications first, implementation second**
- `constitution.md` enforces project-wide rules
- Works with Copilot, Claude, Cursor, and other AI tools
- Released by GitHub in September 2025

---
