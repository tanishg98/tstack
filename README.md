# Claude Code Builder Kit

> A structured set of Claude Code skills, agents, and rules that replaces ad-hoc AI prompting with a repeatable engineering workflow.

Built by [Tanish Girotra](https://github.com/tanishg98) · MIT License

---

## The Problem

Claude Code is powerful out of the box — but without structure, every session starts from scratch. You get inconsistent output, half-finished implementations, no quality gates, and the same corrections over and over.

This kit fixes that. It gives Claude a **defined role for every phase of a build** — planning, execution, code review, self-correction — with explicit handoffs and quality checks between them.

---

## How It Works

Every project follows the same loop:

```
/explore → /createplan → /execute → /review → /reflect
```

| Phase | Skill/Agent | What happens |
|-------|-------------|-------------|
| Understand | `/explore` | Maps the codebase, traces data flows, surfaces risks before any code is written |
| Plan | `/createplan` | Produces a scoped, step-sized implementation plan — no code yet |
| Build | `/execute` | Implements one step at a time, updates the tracker, writes a status report after each |
| Review | `review` agent | Audits the output — severity-classified findings, every issue labeled AUTO-FIX or ASK |
| Learn | `/reflect` | When something goes wrong, traces the failure and applies a surgical fix to the rule/skill that caused it |

---

## Skills

### Planning & Execution

**`/createplan`**
Creates a structured implementation plan before any code is written. Scopes the work into steps that are each completable in one session. Flags risks and open questions before they become bugs.

**`/execute`**
Implements the plan one step at a time. After each step: marks it complete, writes a status report, and stops — so you stay in control of what gets built and when.

**`/explore`**
Reads the codebase autonomously before any feature work. Returns a structured report covering affected files, integration points, constraints, and open questions. Run this before every `/createplan`.

### Building

**`/browser-extension-builder`**
Full lifecycle for Chrome and Firefox extensions (Manifest V3):
- Architecture decision: popup / content script / service worker / options page — only what's needed
- Correct patterns for message passing, `chrome.storage`, CSP-safe HTML
- Security checklist: permission minimisation, shadow DOM isolation, no `eval()`
- Self-test checklist and packaging instructions

**`/mobile-app-builder`**
Two paths depending on what you're building:
- **PWA** — turns any web project into an installable mobile app: `manifest.json`, service worker, safe area insets, touch targets, iOS Safari fixes
- **Expo / React Native** — full scaffold for apps needing native device APIs, with navigation, platform-specific patterns, and build instructions

### Code Quality

**`/reflect`**
Self-correction skill. Auto-triggers when something goes wrong or you push back on Claude's output. Traces the failure to the specific rule or skill that caused it, proposes a precise fix, and applies it only after your approval. The kit gets better over time.

**`/peer-review`**
Triages feedback from a human reviewer. Classifies each finding as Accept, Accept with wrong fix, Context Missing, or Reject — then produces a prioritised action plan.

**`/learning`**
Teaching mode. Explains any concept, pattern, or decision in the codebase across three progressive levels of depth. Pauses for confirmation between each.

### Project Utilities

**`/create-issue`** — Captures a bug or feature as a clean, structured issue document mid-flow without losing context.

**`/documentation`** — Updates `CHANGELOG.md` and inline docs after a feature or fix is implemented.

---

## Agents

Agents run autonomously on a focused task and return a structured report. They never modify files — they only read, analyse, and respond.

**`review`**
Focused code review. Checks logging, error handling, TypeScript, security, React hooks, performance, and architecture. Every finding gets a severity (CRITICAL / HIGH / MEDIUM / LOW) and every CRITICAL/HIGH gets a concrete fix. Findings are classified as AUTO-FIX (mechanical, apply immediately) or ASK (judgment required, confirm first).

**`explore`**
Codebase exploration. Given a feature or bug, reads the relevant files, traces the data flow, identifies integration points and constraints, and returns an exploration report with numbered open questions. Feeds directly into `/createplan`.

---

## Rules (Always On)

Rules load automatically in every session and apply to all output — no invocation needed.

**`builder-ethos`** — The four principles behind every build:

1. **Boil the Lake** — AI makes completeness cheap. Full implementation every time, not 90%.
2. **Search Before Building** — Three layers: tried-and-true patterns → new-and-popular (scrutinise) → first principles (most valuable).
3. **Fix-First Review** — Every finding is AUTO-FIX or ASK before any action is taken.
4. **No AI Slop** — Explicit ban list of the patterns that signal low-craft AI output.

**`code-standards`** — Type safety, comment discipline, and pattern consistency. Comments explain *why*, not what. No `any` types without justification. New code follows the closest existing pattern in the codebase.

---

## Installation

Copy the `.claude/` folder into any project:

```bash
git clone https://github.com/tanishg98/claude-builder-kit
cp -r claude-builder-kit/.claude your-project/.claude
```

Or pick individual skills:

```bash
# Just the planning workflow
cp -r claude-builder-kit/.claude/skills/createplan your-project/.claude/skills/
cp -r claude-builder-kit/.claude/skills/execute your-project/.claude/skills/
cp -r claude-builder-kit/.claude/agents/explore.md your-project/.claude/agents/

# Just the browser extension builder
cp -r claude-builder-kit/.claude/skills/browser-extension-builder your-project/.claude/skills/
```

Then open Claude Code in your project — the skills are immediately available as `/skill-name` commands.

---

## Repo Structure

```
.claude/
├── agents/
│   ├── explore.md                    — codebase exploration
│   └── review.md                     — severity-classified code review
├── rules/
│   ├── builder-ethos.md              — four core engineering principles
│   └── code-standards.md             — types, comments, patterns
└── skills/
    ├── browser-extension-builder/    — Chrome/Firefox MV3 extensions
    ├── mobile-app-builder/           — PWA + Expo/React Native
    ├── createplan/                   — implementation planning
    ├── execute/                      — step-by-step execution
    ├── reflect/                      — self-correction loop
    ├── learning/                     — teaching mode
    ├── documentation/                — changelog + inline docs
    ├── create-issue/                 — issue capture
    ├── peer-review/                  — triage reviewer feedback
    └── explore/                      — codebase exploration (skill version)
```
