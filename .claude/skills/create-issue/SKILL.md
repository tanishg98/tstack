---
name: create-issue
description: Captures a bug, feature, or improvement as a clean, structured issue document. Invoke mid-flow when the user wants to log something without losing their current context.
argument-hint: [brief description of the issue]
---

The user is mid-flow and wants to capture a bug, feature, or improvement fast. Turn their rough description into a clean, complete issue — quickly.

---

## Speed rule

**If the description gives you enough to work with, write the issue immediately.** Don't ask questions for the sake of it. The user is mid-development and every back-and-forth costs them context.

If something critical is genuinely missing, ask — but max one round, max 2-3 questions, then write regardless of what you get back.

---

## What to produce

```markdown
## [Type: Bug | Feature | Improvement | Chore] — [Title]

**Priority:** P0 (critical) | P1 (high) | P2 (normal) | P3 (low)
**Effort:** XS | S | M | L | XL

---

### What's happening / What's needed
[1-3 sentences. For bugs: what's broken. For features/improvements: what's missing or could be better.]

### Expected behaviour
[What should happen instead. Skip for chores.]

### Relevant files
- `src/path/to/file.ts` — [why it's relevant]
- *(max 3)*

### Notes / Risks
- [Edge cases, dependencies, rollback concerns. Skip if none.]
```

---

## Label definitions

**Type:** `Bug` broken | `Feature` net new | `Improvement` existing could be better | `Chore` maintenance, no user impact

**Priority:** `P0` blocking now | `P1` high, next sprint | `P2` normal backlog | `P3` nice to have

**Effort:** `XS` under 1h | `S` half day | `M` 1-2 days | `L` 3-5 days | `XL` needs breaking down

---

## Rules

- **Title:** specific and scannable. "Fix nav bug" is bad. "Bell icon badge not resetting after dropdown opens" is good.
- **Relevant files:** only include files you're confident about. If unsure, say "likely" or skip it.
- **Notes/Risks:** only add if there's something genuinely worth flagging. Don't pad.
- **Defaults:** effort M, priority P2 if unclear. State your assumption if defaulting.
