---
name: mindflint-review-en
description: >
  Learning review mode. Scans the conversation for wrong or missed answers, compiles
  a weakness list, and suggests concrete next steps. One-shot output, does not change
  the current teaching mode.
  Trigger: user says "review", "summarize my weak spots", "what don't I understand yet", "/mindflint-review-en".
license: MIT
---

# Learning Review Mode

Scan this conversation, find what the user genuinely doesn't understand, and compile it into an actionable review list. One-shot output — no ongoing mode change.

## What to scan for

- Questions answered incorrectly
- Questions that needed hints to continue
- Concepts stated imprecisely, or two concepts confused with each other
- Conclusions remembered without understanding the underlying reason

## Output format

```
Learning Review
───────────────
[Concept A]
  Status: needs reinforcement
  Break: [exactly which step is unclear]
  Suggestion: [how to fix it, one sentence]

[Concept B]
  Status: needs deepening
  Break: [knows the conclusion but can't explain why]
  Suggestion: [how to deepen it, one sentence]

Overall: [1-2 sentences, what to focus on next]
```

## Rules

- Only list concepts that have problems — don't list what the user got right (not this skill's job).
- Suggestions must be concrete and actionable — not "review more" level vague.
- If the conversation is too short / not enough data: say directly "Not enough data yet — use `/mindflint-en` or `/mindflint-check-en` first to build it."
- No scores, no rankings.

## Boundaries

One-shot output; return to the previous mode after.  
Continue learning → `/mindflint-en`  
Take a quiz → `/mindflint-check-en`
