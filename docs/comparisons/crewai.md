# Tanker vs CrewAI

[CrewAI](https://github.com/crewAIInc/crewAI) is a Python framework for orchestrating role-playing autonomous AI agents. Agents have roles, goals, backstories, and collaborate as a "crew." Tanker is a different shape: a Claude Code-native product-build pipeline with two human gates.

## TL;DR

- **Tanker is a deployment-aware pipeline. CrewAI is a generic role-playing framework.**
- **Tanker ends at a live URL. CrewAI ends at crew output.**
- **Tanker has opinions baked in. CrewAI is taste-neutral by design.**

---

## What you get with Tanker that you don't get with CrewAI

### 1. **A live URL, not crew output**

Tanker provisions GitHub + Supabase + Vercel + Railway, deploys, smoke-tests, wires Sentry + Plausible + uptime. CrewAI's crew finishes when the agents agree they're done — what happens with the artifacts is your problem.

### 2. **Two human gates pre-qualified by review agents**

Tanker stops twice (post-PRD, post-MVP), and each gate is pre-qualified by a review agent (`prd-reviewer`, `mvp-reviewer`). The owner sees only what's worth seeing. CrewAI's crews don't have a built-in human-checkpoint primitive.

### 3. **34 skills + 9 agents shipped, not assembled**

Tanker comes with the full pipeline: `/grill`, `/benchmark`, `/prd`, `/architect`, `/createplan`, `/execute`, `/ship`, `/deploy`, `/monitor`, plus review and provisioner agents. CrewAI is the kit; you define crews and tasks per project.

### 4. **Resumability + audit trail**

`state.json` checkpointed every phase. `messages.jsonl` typed audit trail with `cause_by` / `sent_from` / `send_to`. `/cto --resume <slug>` works across sessions. CrewAI runs are typically per-process; durability is your job.

### 5. **Cost ceiling enforced**

`--max-cost-usd` halts the run gracefully at the cap. CrewAI has token-usage callbacks; cost ceiling is yours to wire.

### 6. **Local semantic retrieval over your corpus**

brain-index + refs-index, fully local ChromaDB. Memory in CrewAI is optional and unconfigured by default — bring your own.

### 7. **Always-on quality rails**

Tanker's `builder-ethos` rule loads every session with explicit opinions: No AI Slop ban list, Boil-the-Lake completeness, mobile-first responsive, semantic HTML, Safety-Before-Speed for irreversible actions. CrewAI is taste-agnostic.

### 8. **Real shipped product as proof**

Persona Studio ([persona-studio-lime.vercel.app](https://persona-studio-lime.vercel.app)) was built end-to-end with `/cto`. Tanker has the receipts.

---

## Side-by-side

| | CrewAI | Tanker |
|---|---|---|
| **Primary abstraction** | Crew + Agent + Task | Skill + Agent + Rule |
| **Agent definition** | Role + goal + backstory + tools | YAML frontmatter + prompt body + JSON schema sidecar |
| **Process model** | Sequential / hierarchical | DAG with HITL gates + bounded retry loops + pub-sub subscriptions |
| **Memory** | Optional, multiple backends, unconfigured by default | brain-index (ChromaDB) + per-project `.claude/brain.md` + auto-memory |
| **Deployment** | None | gh + supabase + vercel + railway provisioner agents |
| **Human gates** | None built-in | Two mandatory gates pre-qualified by review agents |
| **Cost tracking** | Token callbacks (you wire the cap) | `--max-cost-usd` ceiling + per-Message cost in `messages.jsonl` |
| **Audit trail** | LLM logs | Typed Message envelope per artifact |
| **Distribution** | Pip install | Drop `.claude/` into any project |
| **License** | MIT | MIT |

---

## When CrewAI is the better choice

- You want a **Python framework** for custom agent crews.
- You're comfortable wiring **deployment + quality + memory** yourself.
- You need **multi-provider LLM** support.
- You're working **outside Claude Code**.

## When Tanker is the better choice

- You're on **Claude Code** and want to stop reset-friction every session.
- You want all the deployment + memory + quality rails **wired**, not your problem.
- You want **human gates** without owner attention burn.
- You want a **deployed URL** at the end, not a crew transcript.

---

Both MIT. Pick the shape that matches what you ship.
