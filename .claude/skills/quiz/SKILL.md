---
name: quiz
description: Tests understanding of the current topic with questions. Includes multiple choice, code output prediction, and conceptual questions. Use when user wants to verify their understanding.
---

# Quiz Mode

## Purpose

Test and reinforce understanding through varied question types that challenge different aspects of knowledge.

## When This Activates

- User says "quiz me", "test my understanding"
- `/quiz [topic]`
- "Do I understand this correctly?"
- "Check my knowledge"

## Question Types

### 1. Multiple Choice
- Concept understanding
- Best practices
- Common pitfalls

### 2. Code Output
- "What does this print?"
- Predict behavior
- Trace execution

### 3. Spot the Bug
- Find error in code
- Explain why it fails
- Fix the issue

### 4. Fill in the Blank
- Complete the code
- Missing syntax/concept

### 5. Conceptual
- Explain in your own words
- Compare and contrast
- When would you use X vs Y?

## Response Format

```markdown
### 📝 Quiz: [Topic]

**Question 1** (Multiple Choice)
What is the output of this code?
\`\`\`[language]
// code here
\`\`\`
A) ...
B) ...
C) ...
D) ...

---

**Question 2** (Spot the Bug)
This code has a bug. Find and explain it:
\`\`\`[language]
// buggy code
\`\`\`

---

**Question 3** (Conceptual)
In your own words, explain why [concept] is important.

---

**Question 4** (Code Output)
Trace through this code. What is the final value of `x`?
\`\`\`[language]
// code to trace
\`\`\`

---

### Ready for answers?
Say "check answers" when you've attempted all questions!
```

## Answer Reveal Format

```markdown
### ✅ Quiz Answers

**Q1:** [Correct answer] - [Explanation]

**Q2:** [Bug explanation] - [Fixed code]

**Q3:** [Key points to include in good answer]

**Q4:** [Step-by-step trace]

---

**Your Score:** Guide user to self-assess

**Review:** [Topics to revisit if struggled]
```

## Guidelines

- Mix question types for variety
- Progress from recall → application → analysis
- Include "trick" questions for common misconceptions
- Wait for user to attempt before revealing answers
- Provide detailed explanations, not just correct answers
- Suggest follow-up study for missed concepts
