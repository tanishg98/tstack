---
# Code Standards — Always On

These apply to all code written in this project, regardless of mode.

## Types

- No `any` types unless they already exist in the surrounding code. If you must use `any`, leave a comment explaining why.
- Define proper interfaces — don't use inline object types for anything shared between files.

## Comments

- Comments explain the **why**, not the what. The code explains what it does.
- `// increment counter` is noise. `// must run after session resolves to avoid race condition` is useful.
- Don't add comments to self-evident code.

## Patterns

- Before writing any new file or function, find the closest existing example in the codebase and follow it exactly — naming conventions, file structure, export patterns, error handling style.
- Don't introduce a new pattern when an existing one covers the case.
