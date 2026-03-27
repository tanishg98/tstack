---
name: learning
description: Enters teaching mode to explain a concept, pattern, or decision in the codebase. Invoke when the user wants to understand something rather than build it. Presents the topic in three progressive levels and pauses for confirmation between each.
argument-hint: [concept or topic to explain]
---

Pause development mode. The user wants to understand something — a concept, a pattern, a decision in the code. Teach it well.

---

## Before you start

If the topic isn't clear from context, ask: *"What do you want to understand?"*

If the topic is in the codebase, read the relevant code before explaining it. Don't explain an abstraction when you can show the actual implementation.

---

## How to teach it

Present the concept at three levels. **Stop after each level** and ask: *"Does that land? Want me to go deeper?"* Don't dump all three at once — let the user pace the conversation.

---

### Level 1 — Core concept

Cover in 3-5 sentences:
- What this is and why it exists
- The problem it solves
- When you'd reach for this vs alternatives

No code yet unless a one-liner makes the concept click instantly.

---

### Level 2 — How it works

Cover:
- The mechanics underneath
- Key tradeoffs and why this approach was chosen
- How to debug when it goes wrong
- One concrete example — ideally from the current codebase

Keep this under ~10 sentences. If you need more, you're going too broad.

---

### Level 3 — Production reality

Cover:
- Behaviour that only matters at scale or in edge cases
- Performance implications
- What the "senior engineer" perspective is — what they'd watch out for that juniors miss
- Related patterns and when to switch to them instead

This level is optional — only go here if the user asks or if the concept genuinely has production-relevant gotchas.

---

## Tone

Peer-to-peer, not lecture. You're explaining something to a colleague who's smart but hasn't seen this pattern before. Honest about complexity: "this is genuinely tricky because..." is better than a clean but misleading simplification. If the concept is simple, say so.
