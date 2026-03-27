---
name: createplan
description: Creates a structured implementation plan document. Invoke after /explore Q&A is complete and all ambiguities are resolved. Produces a markdown plan with steps, subtasks, key decisions, and risks.
---

Based on everything discussed during `/explore` and our Q&A, produce a plan document.

The plan is a shared artefact — it's how we track what's being built, in what order, and why. It is **not** the time to write code. Clarity and brevity matter more than completeness.

---

## What to produce

A markdown plan document using the template below. Fill every section — don't skip any, don't add sections that aren't in the template.

**Step sizing:** Each step should be roughly one session's worth of work — focused enough to complete and verify in one sitting, large enough to deliver something meaningful. If a step feels too big, split it. If two steps feel inseparable, merge them.

**Scope discipline:** Only include what was explicitly discussed and agreed during exploration. If something seems obviously useful but wasn't discussed, put it in "Out of Scope" — not in the tasks.

---

## Template

```markdown
# [Feature Name] — Implementation Plan

**Status:** `0% complete` — update this manually as steps finish.

---

## What we're building
One or two sentences. What does this add or fix, and why does it matter?

---

## Out of Scope
Things we are explicitly NOT building in this plan. List them so there's no ambiguity later.
- ...

---

## Key Decisions
Architectural and implementation choices locked in during exploration. For each, note what was chosen, what was considered and rejected, and why.

| Decision | Chosen | Rejected | Reason |
|----------|--------|----------|--------|
| ... | ... | ... | ... |

---

## Steps

### 🟥 Step 1: [Name]
**Goal:** One sentence — what does completing this step achieve?
**Files touched:** `src/path/to/file.ts`, `supabase/migrations/...`

- 🟥 Subtask A
- 🟥 Subtask B
- 🟥 Subtask C

---

### 🟥 Step 2: [Name]
**Goal:** ...
**Files touched:** ...

- 🟥 Subtask A
- 🟥 Subtask B

---

## Risks & Watch-outs
Anything flagged during exploration that could cause problems. Keep this visible — don't bury it.
- ...
```

---

## Status key

| Emoji | Meaning |
|-------|---------|
| 🟥 | To do |
| 🟨 | In progress |
| 🟩 | Done |

Update step and subtask emojis as work progresses. Update the top-line status percentage when a step completes.

---

Once the plan is reviewed and agreed, move to `/execute`.
