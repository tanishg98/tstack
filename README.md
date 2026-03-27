# Claude Code Builder Kit

A production-grade collection of Claude Code skills, agents, and rules that turns Claude into a specialist engineering team — capable of building static websites, browser extensions, and mobile apps with proper quality gates at every step.

Built and maintained by [Tanish Girotra](https://github.com/tanishg98).

---

## What This Is

Claude Code supports custom `/skills`, agents, and always-on rules via `.claude/` configuration files. This repo is a curated set of those files, designed around one goal: **produce complete, polished output — not vague drafts**.

Every skill follows the same pattern:
- Clear trigger conditions so Claude knows exactly when to use it
- Phased workflow with explicit handoffs
- A mandatory eval/quality gate before delivery
- No AI slop, no shortcuts

---

## Skills

### `/static-site-replicator`
Replicates any reference website as a polished static HTML/CSS/JS site with new brand assets. Full lifecycle: screenshot reference → catalog design patterns → build → self-eval → deliver.

### `/browser-extension-builder`
Builds Chrome and Firefox extensions (Manifest V3). Covers architecture decisions, service worker patterns, message passing, permissions auditing, CSP checklist, and packaging. Outputs a ready-to-load unpacked extension.

### `/mobile-app-builder`
Two-path mobile builder:
- **PWA path** — adds `manifest.json`, service worker, and mobile-specific CSS to any static site. Installable on iPhone/Android from the browser.
- **Expo path** — full React Native scaffold for apps that need native device APIs or app store distribution.

### `/createplan`
Creates a structured implementation plan before any code is written. Produces a markdown plan with steps, subtasks, key decisions, and risks.

### `/execute`
Implements a plan step by step, updating the tracker after each step and writing a status report.

### `/explore`
Autonomous codebase exploration — reads files, traces data flows, identifies affected code, returns a structured report. Always run before planning.

### `/reflect`
Self-improvement skill. Triggered when something goes wrong or the user gives corrective feedback. Traces the failure, proposes a surgical fix, applies it after approval.

### Other utilities
`/create-issue` · `/documentation` · `/learning` · `/peer-review`

---

## Agents

### `site-eval`
Pre-launch quality gate for static websites. Audits 8 dimensions: section completeness, visual hierarchy, brand consistency, animation quality, responsiveness, images, technical hygiene, performance — plus **AI slop detection**. Returns PASS / CONDITIONAL / FAIL.

### `review`
Focused code review with severity levels (CRITICAL / HIGH / MEDIUM / LOW). Fix-First classification: every finding is labeled AUTO-FIX or ASK before action is taken.

### `explore`
Codebase exploration agent. Called before any plan or feature — reads files, maps dependencies, returns a structured exploration report.

---

## Rules (Always On)

### `builder-ethos`
Four core principles applied to every build:
1. **Boil the Lake** — completeness is cheap with AI. The full implementation, always.
2. **Search Before Building** — three-layer knowledge model (tried-and-true → new-and-popular → first principles).
3. **Fix-First Review** — every finding classified as AUTO-FIX or ASK before action.
4. **No AI Slop** — explicit ban list of patterns that signal low-craft AI output.

### `static-site-standards`
Architecture, CSS, animation, image, and HTML rules for every static site. Includes the AI slop ban list and the eval gate requirement.

### `code-standards`
Type safety, comment discipline, and pattern-following rules for all code written in this project.

---

## Installation

Clone into your project's `.claude/` folder, or copy individual skill/agent files:

```bash
# Clone the full kit into a project
git clone https://github.com/tanishg98/claude-builder-kit .claude-builder

# Or copy a specific skill
cp -r .claude-builder/.claude/skills/static-site-replicator ~/.claude/skills/
```

Or reference directly in your own project's `.claude/` directory structure:

```
your-project/
└── .claude/
    ├── skills/
    │   └── static-site-replicator/
    │       └── SKILL.md
    ├── agents/
    │   ├── site-eval.md
    │   └── review.md
    └── rules/
        ├── builder-ethos.md
        └── code-standards.md
```

---

## The AI Slop Ban List

Patterns this kit actively prevents — the fingerprints of low-craft AI-generated UI:

- Purple/violet/indigo gradient as primary palette (`#6366f1–#8b5cf6`)
- 3-column feature grid: icon-in-colored-circle + bold title + 2-line description
- Generic hero copy: "Welcome to X", "Unlock the power of...", "Streamline your workflow"
- `text-align: center` on more than 60% of text containers
- Inter as the sole font choice
- Missing hover/focus states on interactive elements
- `height: 100vh` (breaks on iOS Safari — should be `min-height: 100dvh`)
- `outline: none` without a visible focus replacement

The `site-eval` agent checks for every one of these before a site is approved for delivery.

---

## Repo Structure

```
.claude/
├── agents/
│   ├── explore.md          — codebase exploration
│   ├── review.md           — code review with severity levels
│   └── site-eval.md        — pre-launch quality gate
├── rules/
│   ├── builder-ethos.md    — core engineering principles
│   ├── code-standards.md   — types, comments, patterns
│   └── static-site-standards.md — always-on web rules
└── skills/
    ├── static-site-replicator/
    ├── browser-extension-builder/
    ├── mobile-app-builder/
    ├── createplan/
    ├── execute/
    ├── explore/   (deprecated — use agent)
    ├── reflect/
    ├── learning/
    ├── documentation/
    ├── create-issue/
    └── peer-review/
```

---

## License

MIT — use freely, attribution appreciated.
