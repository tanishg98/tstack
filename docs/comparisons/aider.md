# Tanker vs Aider

[Aider](https://github.com/Aider-AI/aider) is an in-terminal AI pair programmer — edits code in your repo with git-aware diffs. Tanker is a different shape: a Claude Code-native product-build pipeline.

## TL;DR

- **These are complementary, not competitive.** Use Aider for line-level pair-programming. Use Tanker for new-product autopilot or substantial feature work.
- **Tanker ends at a deployed URL. Aider ends at a commit.**
- **Tanker has a PRD/architect/plan phase. Aider assumes you already know what to build.**

---

## What you get with Tanker that you don't get with Aider

### 1. **A full pipeline, not just edits**

Tanker runs the entire flow: `/grill` → `/benchmark` → `/prd` → `/architect` → `/createplan` → `/execute` → `/ship` → `/deploy` → `/monitor`. Aider focuses on the `/execute` step — useful, but only one phase of the work.

### 2. **PRD, architecture, and plan as artifacts**

`/prd` produces an exhaustive PRD with HTML wireframes for every screen. `/architect` produces a system design with components + API contracts + data model + decision log. `/createplan` produces a risk-first plan with verify gates. Aider has no equivalent — it assumes the design is already in your head.

### 3. **Two human gates with pre-qualification**

Tanker pauses at PRD and MVP, each gated by a review agent before reaching you. Aider has no gate model — every edit goes through, you commit when you decide.

### 4. **Real provisioning + deployment + monitoring**

`/cto` provisions GitHub + Supabase + Vercel + Railway, deploys, smoke-tests, wires monitoring. Aider edits code in an existing repo; deployment is your problem.

### 5. **Multi-agent specialization**

Tanker dispatches frontend / backend / data / content engineers as parallel subagents, each with their own memory slice (via brain-index `--domain` filter). Aider is single-threaded — one agent, one conversation.

### 6. **Cost ceiling**

`--max-cost-usd` halts at the cap. Aider shows token usage in the TUI but doesn't enforce a ceiling.

### 7. **Always-on quality rails**

`builder-ethos` rules load every session — No AI Slop ban list, Boil-the-Lake completeness, Safety-Before-Speed. Aider trusts your prompts.

---

## Side-by-side

| | Aider | Tanker |
|---|---|---|
| **Primary use case** | Edit code in an existing repo | Build a new product from a brief (or add a substantial feature) |
| **Interaction model** | Conversational, edit-by-edit | Phased pipeline with human gates |
| **Phases** | One — the edit | Eleven — `/grill` through `/monitor` |
| **Git integration** | Native (commits, diffs, revert) | Via `/ship` skill |
| **PRD / architect / plan** | No | Yes — exhaustive artifacts before code |
| **Provisioning** | No | gh + supabase + vercel + railway |
| **Quality gates** | Linting/tests if configured | pre-merge + autoresearch + prd-reviewer + mvp-reviewer |
| **Multi-LLM** | Yes (multi-provider) | Claude Code-native |
| **Cost tracking** | Token meter in TUI | `--max-cost-usd` ceiling + per-Message audit |

---

## When Aider is the better choice

- You have a **clear, contained coding task** in an existing repo.
- You want **fast TUI in-terminal edits** with native git diffs.
- You want **multi-provider** LLM support.
- You're **not on Claude Code** (Aider works in any terminal).

## When Tanker is the better choice

- You're starting a **new product** from a brief.
- You want PRD + architecture + plan + build + deploy + monitor **wired**.
- You want **two human gates** at the high-leverage decisions.
- You're already on **Claude Code**.

---

## Use both

The two compose well:

- **Tanker (`/cto`)** for new builds — sets up the repo, schema, scaffolds the product.
- **Aider** for tight inner-loop edits inside the resulting Tanker-built repo — its TUI pace is hard to beat for line-level work.

You don't have to choose.
