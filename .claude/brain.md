# Project Brain — tstack

> Auto-maintained by /learn. Read this before starting any session on this project.
> Last updated: 2026-04-22

---

## Conventions
*How this project does things: naming, file structure, patterns, tech choices.*

- Skills live at `.claude/skills/[name]/SKILL.md` with YAML frontmatter (`name`, `description`, `triggers`, `args`), followed by phases, output format, and rules sections
- Always-on rules live in `.claude/rules/` (builder-ethos, code-standards, static-site-standards)
- Agents live in `.claude/agents/[name].md`
- All skill output goes to `outputs/[project-name]/`
- Every skill ends with a handoff line pointing to the next skill in the chain

---

## Decisions
*Why key choices were made. Prevents re-litigating settled questions.*

- **Kit name is tstack:** Renamed from `my-builder-kit` / `claude-builder-kit` to `tstack` — *Reason: personal parallel to gstack (Garry Tan), repo is `tanishg98/tstack`*
- **Three-layer memory system:** Global `~/.claude/CLAUDE.md` rule auto-reads `.claude/brain.md` + `.claude/context.md` + `.claude/git-state.md` at every session start — *Reason: CLI sessions start blank; CLAUDE.md is the only thing that auto-loads*
- **Stop hook for git-state:** `~/.claude/settings.json` Stop hook runs `sync-memory.sh` async on every turn, writes `.claude/git-state.md` — *Reason: always-current branch/commit without manual /context-save*
- **`/remember inject` is semi-manual:** Injects brain into CLAUDE.md for machines without the global rule — *Reason: requires Claude to summarize brain.md, can't be done in a shell script*
- **Skill chaining via SKILL.md instructions:** Skills end with explicit handoff lines; Claude follows them using the Skill tool — *Reason: no native platform chaining mechanism; instruction-following is the only approach*
- **`/ui-hunt` is tstack's unique differentiator:** Finds best-in-class products before building so designs have a real reference — *Reason: root cause of AI slop is building without a reference*

---

## Pitfalls
*What broke, what was confusing, what not to repeat.*

- **`advisor` skill mismatch:** When user says "use advisor to review", they likely mean strategic advice — but the advisor skill is specifically for building Claude API apps with the Opus+Sonnet advisor tool pattern. Clarify before invoking.

---

## Preferences
*How the project owner wants things done. These override defaults.*

- After adding or modifying any skill/rule/agent, always commit and push to `tanishg98/tstack`
- Naming suggestions for this project should be personal — TG / Tanish-inspired (like tstack parallels gstack)
- Keep skill SKILL.md files concise and structured — phases, not walls of text
