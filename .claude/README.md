# `.claude/` — Tanker source of truth

This is the Tanker framework. Everything Claude Code reads at runtime lives here.

```
.claude/
├── skills/          — 34 slash commands. /cto, /grill, /prd, /architect, /analyst, …
├── agents/          — 9 specialists. pre-merge, prd-reviewer, mvp-reviewer, *-provisioner
├── rules/           — 3 always-on operating principles loaded every session
├── schemas/         — JSON schemas validating skill/agent outputs
├── tanker.yaml      — multi-model preferences + cost ceilings + retry caps
└── brain.md         — project-scoped memory (per-repo, written by /learn)
```

## How Claude Code uses these

- **Skills** (`skills/<name>/SKILL.md`) appear as `/skill-name` in any project that has Tanker installed. Each has YAML frontmatter (name, description, triggers, args) and a prompt body.
- **Agents** (`agents/<name>.md`) are spawned via the Agent tool in isolated context. Each declares `tools` and `model`; some declare `subscribes_to` for the pub-sub orchestrator pattern.
- **Rules** (`rules/*.md`) auto-load every session. They encode opinions: builder-ethos, code-standards, static-site-standards.
- **Schemas** (`schemas/*.schema.json`) validate the JSON sidecars produced by skills like `/prd`, `/architect`, `/createplan`. Reviewer agents read JSON, not parse markdown.

## Editing

Drop new skills under `skills/<name>/SKILL.md`. Drop new agents under `agents/<name>.md`. Always-on rules go under `rules/`. The framework picks them up on next session.

For full conventions and contribution bar, see [CONTRIBUTING in the README](../README.md#contribute).
