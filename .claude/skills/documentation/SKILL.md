---
name: documentation
description: Updates CHANGELOG.md and inline docs to reflect a recent code change. Invoke after implementing a feature or fix — describe what changed or paste the diff.
argument-hint: [description of what changed or paste diff]
---

Update the documentation to reflect recent code changes. Fast, accurate, no fluff.

---

## Step 1 — Understand the change

The user will describe what changed, or paste a diff. If neither is provided, ask: *"What changed? Paste the diff or describe it."*

Don't proceed until you know what changed.

---

## Step 2 — Read before you write

Before touching any documentation:
- Read the actual code for any file mentioned in the change
- If existing docs describe behaviour in that file, check whether they still match
- If they don't match: update the docs to match the code, not the other way around

**The code is truth. Docs are descriptions of the code.**

---

## Step 3 — Update CHANGELOG.md

Add an entry under `## [Unreleased]` (create the section if it doesn't exist).

```markdown
## [Unreleased]

### Added
- [User-facing description of net-new feature or capability]

### Changed
- [User-facing description of changed behaviour or interface]

### Fixed
- [User-facing description of bug that was fixed]

### Security
- [Security fix description — include CVE if applicable]

### Removed
- [What was removed and what to use instead]
```

Only include categories that apply.

**Entry rules:**
- One line per change, starting with a verb: "Add", "Fix", "Remove", "Update"
- User-facing language — describe what the user sees, not the internal implementation
- Omit internal refactors, variable renames, and code cleanups unless they affect behaviour or API

---

## Step 4 — Update inline docs (only if broken)

If the change modified the signature, behaviour, or usage of a function/component that has a JSDoc or inline comment, update that comment. Don't touch docs that are still accurate.

Do not add JSDoc to functions that don't have it.

---

## Rules

- **Don't invent changelog entries.** Only document what actually changed.
- **Don't pad.** One line per change.
- **Ask once if intent is unclear.** If you can't tell whether a change is user-facing or internal, ask before writing.
- **No "Improvements to internal architecture."** If it's internal, it doesn't go in the changelog.
