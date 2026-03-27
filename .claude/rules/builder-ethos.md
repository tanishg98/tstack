---
# Builder Ethos — Always On

Core engineering principles for everything built in this project. Adapted from gstack (garrytan/gstack).

---

## Principle 1: Boil the Lake

> AI-assisted coding makes the marginal cost of completeness near-zero. When the complete implementation costs minutes more than the shortcut — do the complete thing. Every time.

**What "boilable" means here:**
- 100% of the spec implemented (not 90% "to ship faster")
- All edge cases handled at the point of implementation, not deferred
- Accessible markup (alt text, ARIA labels, keyboard nav) on every element
- Mobile-responsive from the first commit, not retrofitted
- `prefers-reduced-motion`, `prefers-color-scheme` respected from the start

**Anti-patterns to reject:**
- "This covers 90% of cases — good enough"
- "We can add mobile support in a follow-up"
- "Let's defer error states to later"

**The compression table — always show this when a task feels too big:**

| Task type | Human team | AI-assisted |
|-----------|-----------|-------------|
| Boilerplate / scaffolding | 2–4 hours | 5 minutes |
| UI components from spec | 1–2 days | 30 minutes |
| Full feature implementation | 1 week | 2–3 hours |
| Bug fix with root cause | 2–4 hours | 20 minutes |
| Architecture / design | Days | Still days (thinking is the bottleneck) |

The "it would take too long" objection almost never holds for frontend/static work. Evaluate honestly before deferring.

---

## Principle 2: Search Before Building

Before writing any non-trivial piece of code, understand what everyone else is doing and why — then reason from first principles about whether it's the right approach.

**Three-layer knowledge model:**

**Layer 1 — Tried and true** (established patterns)
Standard patterns used by the whole industry. Cheap to verify, occasionally worth questioning. Default to these unless there's a clear reason not to.
Example: CSS Flexbox for layouts, Intersection Observer for scroll animations.

**Layer 2 — New and popular** (trending approaches)
Search before using. Crowds can be wrong. Ask: is this popular because it's genuinely better, or because it's new? Scrutinize before adopting.
Example: a new CSS animation library everyone's using — check if vanilla CSS does the job.

**Layer 3 — First principles** (original reasoning)
The most valuable layer. After surveying what everyone does and why, ask: what does *this specific problem* actually require? Is the conventional approach wrong here?
Example: A listing site with 50 items doesn't need a JS framework. The conventional "use React" answer may be wrong.

**The Eureka Moment:**
The most valuable outcome of searching is not finding a solution to copy.
It is: understanding what everyone is doing AND WHY, then discovering that the conventional approach has an assumption that's wrong for your case.

---

## Principle 3: Fix-First Review

When reviewing code (your own or in a review agent), classify every finding before acting:

**AUTO-FIX** (do it, no discussion needed):
- Dead code / unused variables
- Missing `alt` text, `loading="lazy"`, `defer`/`async` on scripts
- Hardcoded values that should be CSS custom properties
- `console.log` left in
- Picsum placeholder URLs remaining in delivery
- Missing semantic HTML tags (can infer correct element)
- Inconsistent spacing/padding where the pattern is clear

**ASK** (show the issue, propose the fix, wait for approval):
- Anything changing visual appearance significantly
- Layout changes affecting multiple sections
- Anything touching animation timing or interaction design
- Removing a section or element that may be intentional
- Security-relevant changes (CSP headers, permissions, input handling)
- Any fix longer than ~20 lines

Rule of thumb: if a senior developer would apply it without discussion in a 5-minute review, it's AUTO-FIX. If it requires a judgment call, ASK.

---

## Principle 4: No AI Slop

AI-generated UIs have recognisable fingerprints that signal low craft. Never produce these patterns:

- Purple/violet/indigo gradients (`#6366f1–#8b5cf6` range) as the primary palette
- 3-column feature grid: icon-in-colored-circle + bold title + 2-line description
- `text-align: center` on more than 60% of text containers
- Uniform bubbly border-radius (>80% of elements using 16px+)
- Generic hero copy: "Welcome to X", "Unlock the power of...", "Streamline your workflow"
- More than 3 font families on a page
- Heading levels that skip (h1 → h3 without h2)
- Blacklisted fonts: Papyrus, Comic Sans, Lobster, Impact, Jokerman
- Missing hover/focus states on any interactive element
- `outline: none` without a visible focus replacement
