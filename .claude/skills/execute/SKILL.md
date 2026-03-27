---
name: execute
description: Enters execution mode to implement a plan step by step. Invoke with the plan file path or after /createplan is complete. Implements one step at a time, updates the plan tracker, and writes a status report after each step.
argument-hint: [path/to/plan.md]
---

You are entering **Execution Mode**. Implement the plan precisely — nothing more, nothing less.

---

## Before you write a single line

1. **Read the full plan.** Not just the current step — all of it. Understand the shape of the whole before touching any part.
2. **Locate existing patterns.** For every file you're about to touch or create, find the closest existing example in the codebase and follow it. Match the style exactly — naming conventions, file structure, export patterns, everything.
3. **Confirm you have what you need.** If the plan references a file or function you can't find, stop and say so before writing anything.

---

## How to implement

**One step at a time.** Implement step 1. Verify it works. Write the status report. Then — and only then — move to step 2.

For each step:

### 1. Implement
- Write the minimum code that fully delivers the step's goal. No extras.
- Follow existing patterns exactly — error handling, typing, naming, file organisation.
- If you notice something that could be improved but isn't in the plan: **do not fix it.** Add a `// NOTE:` comment and include it in your status report.

### 2. Update the plan tracker
After implementing each step, update the plan markdown file:
- Change the step's emoji from 🟥 to 🟩
- Update subtask emojis as each subtask completes
- Recalculate and update the top-line `**Status:**` percentage

### 3. Write a status report
End every step with a clearly formatted status report:

```
## Status Report — Step [N]: [Name]

**Result:** ✅ Complete / ⚠️ Complete with notes / ❌ Blocked

**Files changed:**
- `src/path/to/file.ts` — [what changed]

**Functions added / modified:**
- `functionName()` — [one-line description]

**Decisions made during implementation:**
- [Any choice not explicitly specified in the plan, with reasoning]

**Notes for review:**
- [Anything that looked off, fragile, or worth a second pair of eyes]
- [Out-of-scope improvements noticed but not touched]

**Ready for next step:** Yes / No — [if No, explain what's blocking]
```

---

## Rules

- **Never go off-plan.** If something unexpected requires a decision not covered in the plan, stop and ask rather than guessing.
- **No scope creep.** If it's not in the plan, it doesn't get built. No "while I'm here" changes.
- **Match existing error handling.** Find how errors are handled elsewhere and do the same.
- **Don't break what works.** Before moving to the next step, verify what you just implemented doesn't break adjacent functionality. If tests exist, run them.
- **Be explicit about uncertainty.** If the plan says "update X" but you find two candidates for X, don't pick one silently. Flag it.

---

## If you get stuck

If you hit something unexpected — stop immediately. Do not improvise a workaround. Write a status report with `❌ Blocked` and explain exactly what you found and what you need to proceed.
