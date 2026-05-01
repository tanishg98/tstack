# Tanker vs gstack

[gstack](https://github.com/garrytan/gstack) is Garry Tan's Claude Code skill+rule kit. Both gstack and Tanker use Claude Code's same primitives (skills, agents, always-on rules) — but the kits ship with different opinions, different surfaces, and different end states.

## TL;DR

- **Tanker ends at a deployed URL. gstack ends at code.**
- **Tanker is opinionated for product builds. gstack is opinionated for engineering work.**
- Different operator profiles. Pick the one that matches what you ship.

---

## What you get with Tanker that you don't get with gstack

### 1. **`/cto` autopilot orchestrator**

One command runs the whole pipeline: `/cto "<brief>"` → intake → context → reference → grill → benchmark → PRD → architect → plan → provision → build → mvp review → deploy → monitor. gstack chains skills manually; you invoke each step.

### 2. **Real infrastructure provisioning**

Provisioner agents create GitHub repos, Supabase projects, Vercel projects, Railway services via official APIs from a vault at `~/.claude/vault/credentials.json` (0600). gstack stops at code on disk.

### 3. **Two human gates pre-qualified by review agents**

`prd-reviewer` and `mvp-reviewer` agents pre-qualify the artifact before it reaches the human gate. Owner only sees what's worth seeing. gstack has safety primitives (`/careful`, `/freeze`, `/guard`) but no built-in gate-with-pre-qualification pattern.

### 4. **Cost ceiling**

`--max-cost-usd` enforced at the framework level. Tanker tracks spend per Message envelope, halts at the cap. gstack has no in-framework cost ceiling.

### 5. **Resumable + typed audit trail**

`state.json` checkpointed every phase. `messages.jsonl` typed envelope (`cause_by` / `sent_from` / `send_to` / `in_reply_to`). `/cto --resume <slug>` works across sessions. gstack runs are session-scoped.

### 6. **Local semantic retrieval**

`brain-index` indexes your Obsidian vault into local ChromaDB. `refs-index` does the same for curated GitHub repos. `/cto` Phase 1 retrieves from both. gstack has session-checkpoint primitives (`/context-save`, `/context-restore`) but no semantic retrieval over a personal corpus.

### 7. **Per-product-build skills shipped**

`/grill`, `/benchmark`, `/prd`, `/architect`, `/createplan`, `/ui-hunt`, `/design-shotgun`, `/static-site-replicator`, `/analyst` — Tanker ships skills for the whole product-build chain. gstack ships engineering-focused skills.

### 8. **Specialty agents**

`prd-reviewer`, `mvp-reviewer`, `pre-merge`, `site-eval`, `github-scout`, `gh-provisioner`, `supabase-provisioner`, `vercel-provisioner`, `railway-provisioner` — Tanker ships 9 agents. Most are review or provisioning specialists.

### 9. **JSON sidecar contracts for every artifact**

Top skills output JSON validated against schemas in `.claude/schemas/`. Reviewer agents read JSON, not parse markdown. The retry loop decides on `verdict + findings[].fix_kind`, not regex.

### 10. **Real shipped product as proof**

Persona Studio ([persona-studio-lime.vercel.app](https://persona-studio-lime.vercel.app)) is a real product built end-to-end with `/cto`.

---

## What gstack has that Tanker doesn't

Honest:

- **Compiled Chromium binary for browser automation** — Tanker is pure Claude Code, no compiled deps.
- **Dedicated safety primitives** (`/careful`, `/freeze`, `/guard`) as separate skills. Tanker folds safety into the always-on `builder-ethos` Principle 5 ("Safety Before Speed") — same outcome, different surface.

---

## Side-by-side

| | gstack | Tanker |
|---|---|---|
| **Top-level orchestrator** | Manual skill chaining | `/cto "<brief>"` autopilot |
| **End state** | Code on disk | Deployed URL with monitoring |
| **Provisioning** | None | gh + supabase + vercel + railway |
| **Human gates** | Implicit (you decide when to invoke skills) | Two mandatory gates pre-qualified by review agents |
| **Cost ceiling** | None | `--max-cost-usd` (default $10) |
| **Resumability** | `/context-save` + `/context-restore` | `state.json` + typed `messages.jsonl`, `--resume` |
| **Semantic retrieval** | None | brain-index (Obsidian) + refs-index (GitHub) |
| **Skills focus** | Engineering | Product build (think → spec → design → plan → build → ship → monitor) |
| **Browser automation** | Compiled Chromium binary | Pure Claude Code (use Playwright via Bash) |
| **License** | MIT | MIT |

---

## When gstack is the better choice

- You want the **original Garry Tan kit**.
- You don't need autopilot — you prefer **manual skill chaining**.
- You want **dedicated safety skills** (`/careful`, `/freeze`, `/guard`) as separate primitives.
- You need **compiled browser automation** out of the box.

## When Tanker is the better choice

- You want **autopilot** (`/cto` end-to-end).
- You want **provisioning + deployment + monitoring** wired.
- You want **two pre-qualified human gates**.
- You want a **deployed URL** at the end, not code on disk.
- You want **opinionated taste** baked in (No AI Slop ban list, light-mode-for-SMB, mobile-first).

---

Both MIT. Different opinions on the same primitives. Pick the kit that matches your operator model.
