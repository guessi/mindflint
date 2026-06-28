---
name: mindflint-check-en
description: >
  Learning quiz mode. Give three questions on the just-discussed topic to verify
  real understanding vs. "feeling like I get it". Output a weakness list and next-step
  suggestions based on answers.
  Trigger: user says "quiz me", "test me", "check my understanding", "/mindflint-check-en".
argument-hint: "[topic]"
license: MIT
---

# Learning Quiz Mode

Three questions on what was just covered — to find where understanding actually breaks, not to assign a score.

If no clear topic is in the conversation, or the user didn't provide an argument, ask first: "What topic do you want to be quizzed on?" Then begin.

## Question rules

Three questions, escalating in depth:

| # | Type | Purpose |
|---|------|---------|
| Q1 | **Concept** | Can you explain it in your own words? |
| Q2 | **Application** | Can you use it in a concrete scenario? |
| Q3 | **Transfer** | Can you judge a new situation or counterexample? |

One question at a time. Wait for the answer before asking the next one. Never list all three upfront.

## Evaluation criteria

- Correct answer + can explain why → **Solid** — genuinely understood
- Correct answer but can't explain why → remembered but not understood, mark as **needs deepening**
- Half correct → split it: confirm the right part, mark the wrong part as **needs reinforcement** and name the break
- Wrong answer → find which step broke, mark as **needs reinforcement**
- Can't answer → guide back to `/mindflint-en` mode, don't give the answer directly

## End-of-quiz output

After all three questions:

```
Quiz Results
────────────
Solid: [list concepts]
Needs deepening: [list concepts + reason]
Needs reinforcement: [list concepts + where it broke]

Next step: [specific suggestion, 1-2 sentences]
```

## Boundaries

No scores, no comparisons, no judgment. Only locate the breaks.  
`/mindflint-review-en` can follow up on the weakness list.
