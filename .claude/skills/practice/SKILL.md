---
name: practice
description: Generates hands-on coding exercises for the current topic. Provides progressively challenging problems with hints and solutions. Use when user wants to practice and reinforce learning.
---

# Practice Mode

## Purpose

Generate hands-on exercises that reinforce learning through active problem-solving.

## When This Activates

- User says "practice mode", "give me exercises"
- `/practice [topic]`
- "I want to try coding this"
- "Give me a challenge"

## Exercise Structure

### Difficulty Progression
1. **Warm-up** - Apply basic concept directly
2. **Standard** - Combine with other concepts
3. **Challenge** - Solve real-world problem
4. **Boss Level** - Complex, multi-step problem

### Each Exercise Includes
- Clear problem statement
- Input/output examples
- Hints (progressive, not immediate)
- Solution (hidden until requested)

## Response Format

```markdown
### 🎯 Practice: [Topic]

#### Exercise 1: [Name] (Warm-up)
**Problem:** [Clear description]

**Examples:**
- Input: `...` → Output: `...`
- Input: `...` → Output: `...`

**Your task:** Implement in `./lessons/[topic]/exercises/exercise-1.[ext]`

<details>
<summary>💡 Hint 1</summary>
[First hint - direction only]
</details>

<details>
<summary>💡 Hint 2</summary>
[Second hint - more specific]
</details>

<details>
<summary>✅ Solution</summary>

\`\`\`[language]
// Solution with explanation
\`\`\`
</details>

---

#### Exercise 2: [Name] (Standard)
[Same structure...]

---

#### Exercise 3: [Name] (Challenge)
[Same structure...]
```

## Guidelines

- Create exercises that build on the current lesson
- Include test cases user can verify against
- Provide progressive hints (don't give away answer immediately)
- Save exercise files to `./lessons/[topic-slug]/exercises/`
- Encourage user to try before revealing solutions
- Offer to review user's solutions when submitted
