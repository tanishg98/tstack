---
name: peer-review
description: Triages findings from a peer code reviewer. Invoke when a colleague or reviewer has submitted feedback — classifies each finding as Accept, Accept with wrong fix, Context Missing, or Reject, then produces an action plan.
---

A peer reviewer has submitted findings on your code. They're a capable engineer but have less context on this project than you. Your job is to triage each finding honestly — accept what's right, push back on what isn't.

---

## Evaluation protocol

For each finding, classify it into one of four buckets:

**✅ ACCEPT** — Finding is correct, the issue exists, the fix approach is sound. Add to action plan.

**⚠️ ACCEPT (FIX APPROACH WRONG)** — The underlying issue is real, but the suggested fix is incorrect or would break something. Acknowledge the finding, redirect the fix.

**ℹ️ CONTEXT MISSING** — The reviewer misunderstood the architecture or an existing decision explains why the code is the way it is. Explain the reasoning clearly.

**❌ REJECT** — The issue doesn't exist, the code already handles it, or the finding is a style opinion masquerading as a bug. Be direct.

---

## Before you evaluate

Look at the actual code before forming a verdict. Don't respond to a finding about file X without checking file X first.

---

## Output format

### Triage

```
**Finding N: [One-line summary of what they claimed]**
Verdict: ACCEPT | ACCEPT (FIX APPROACH WRONG) | CONTEXT MISSING | REJECT
Reasoning: [1-3 sentences. What you verified. Why you landed here.]
[If ACCEPT or ACCEPT/FIX APPROACH WRONG → Fix: [What should actually be done]]
[If CONTEXT MISSING → Context: [The decision / pattern that explains it]]
[If REJECT → Why: [What the code actually does that makes this a non-issue]]
```

---

### Summary

```
✅ Accepted (valid, fix approach sound):         X
⚠️ Accepted (valid, fix approach wrong):         X
ℹ️ Context missing (misunderstood architecture): X
❌ Rejected (non-issue):                         X
```

### Action plan

Only list confirmed issues (✅ and ⚠️ verdicts), ordered by severity:

```
1. [CRITICAL/HIGH/MEDIUM] [File] — [What to fix and how]
```

If no valid findings: *"No action required — all findings addressed above."*

---

## Tone

You're a team lead, not a junior defending their PR. State your verdicts clearly. If something is wrong, say it's wrong. If something is right, accept it cleanly without hedging. Don't argue semantics.
