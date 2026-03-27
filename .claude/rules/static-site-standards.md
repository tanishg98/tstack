---
# Static Site Standards — Always On

These rules apply whenever a static HTML/CSS/JS website is being built or modified in this project.

## Architecture

- **Single-file first**: Default to one `index.html` with inline `<style>` and `<script>`. Only split into separate files when the project explicitly has multiple pages.
- No frameworks (React, Vue, etc.). Vanilla HTML/CSS/JS only.
- No build tooling (Webpack, Vite, etc.). Files must open directly in a browser.
- External dependencies via CDN only (fonts, icon libraries). Never npm.

## CSS

- Always define CSS custom properties in `:root` at the top of the stylesheet. Colors, fonts, spacing, and max-width must be variables — never hardcoded in rule bodies.
- Mobile-first responsive layout. Every section must work at 375px width minimum.
- Use `clamp()` for fluid type sizing on headings.
- No inline `style=""` attributes except for dynamic values set by JS.

## Animations

- All scroll-triggered animations use `IntersectionObserver` — never `scroll` event listeners.
- Every animated element gets the class `animate` before reveal and `animate visible` after.
- Respect `prefers-reduced-motion`: wrap animation declarations in `@media (prefers-reduced-motion: no-preference)`.
- Keep transition durations between 400ms–700ms. Nothing faster than 300ms, nothing slower than 900ms.

## Images

- All `<img>` tags must have descriptive `alt` text and `loading="lazy"`.
- Use Unsplash direct URLs (`https://images.unsplash.com/photo-[ID]?w=1600&q=80`) as the first choice for stock imagery. Picsum placeholders are only acceptable during initial scaffolding — replace before delivery.
- Never hotlink images from the reference site being replicated.

## HTML

- Use semantic elements: `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`.
- Every page must have: a meaningful `<title>`, a `<meta name="description">`, and `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Add `scroll-behavior: smooth` on `html` for anchor navigation.

## Output

- Finished files go in `outputs/[site-name]/`.
- Every delivered `index.html` must have the builder comment block as the first line inside `<head>`.
- No `console.log` in delivered code.

## No AI Slop

These patterns are the fingerprint of low-craft AI-generated UI. Never produce them:

- Purple/violet/indigo gradients (`#6366f1–#8b5cf6` range) as a primary palette choice
- 3-column feature grid: icon-in-colored-circle + bold title + 2-line description
- `text-align: center` on more than 60% of text containers
- Uniform bubbly border-radius (>80% of elements using the same value ≥16px)
- Generic hero copy: "Welcome to X", "Unlock the power of...", "Streamline your workflow", "Transform your..."
- More than 3 font families on a page
- Heading levels that skip (h1 → h3 without h2)
- Blacklisted fonts: Papyrus, Comic Sans, Lobster, Impact, Jokerman
- Missing hover and focus states on any interactive element (button, link, input)
- `outline: none` without a visible custom focus indicator replacement

## Eval Gate

- No site is "done" until it has passed the `site-eval` agent. If the eval returns FAIL or CONDITIONAL, resolve all blocking issues and re-run before marking complete.
