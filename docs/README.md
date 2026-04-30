# `docs/` — mkdocs site source

Tanker's docs site, served via GitHub Pages from the workflow at `.github/workflows/docs.yml`.

## Structure

```
docs/
├── index.md                      — homepage
├── getting-started/              — install, first /cto run, vault, brain-index
├── architecture/                 — overview, messages, state, gates, provisioners, config, hosted
├── skills/                       — per-skill pages (most point to source SKILL.md files)
├── agents/                       — per-agent pages
├── rules/                        — per-rule pages
├── comparisons/                  — Tanker vs MetaGPT / AutoGen / CrewAI / Aider / gstack
└── faq.md
```

## Build locally

```bash
pip install mkdocs-material
mkdocs serve         # http://127.0.0.1:8000
mkdocs build --strict
```

`--strict` is required before opening a docs PR — broken internal links will fail the build.

## Deploy

Pushes to `main` that touch `docs/`, `mkdocs.yml`, or the workflow file trigger a Pages deployment. First-time setup: enable Pages in repo Settings → Pages → Source: GitHub Actions.

## Convention

- One topic per page. No mega-pages.
- Per-skill / per-agent docs are thin pointers to the source SKILL.md / agent.md files. The source of truth is the runtime artifact, not a duplicate doc.
- Comparison pages must be honest. Credit the other framework where it's stronger; explain Tanker's choice where it diverges.
