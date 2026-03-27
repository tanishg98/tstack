---
name: static-site-replicator
description: >
  Replicate any reference website as a polished static HTML/CSS/JS site with new brand assets.
  Trigger when the user shares a reference website URL and wants a new site built from it,
  says "build me a site like X", "replicate this website", "make a static site based on this design",
  or wants any multi-section informational/listing/marketing website — with or without a reference.
  This skill manages the full build lifecycle: gather → build → eval → refine → deliver.
---

# Static Website Replicator

You are building a polished static website by studying a reference design and adapting it with new brand assets. The result is a self-contained HTML file (or small set of files) that matches the reference site's structure, layout, and feel — with the client's identity applied throughout.

**Before writing any code**, run through Phase 1 fully. Do not skip straight to building.

---

## Phase 1 — Intelligence Gathering

### 1a. Screenshot the reference site thoroughly

Use browser tools to visit the reference URL and capture:
- Full above-the-fold hero
- Each major section (scroll in increments, don't miss anything)
- Footer
- Any hover/interactive states you can reach

As you capture, narrate the **visual vocabulary**: what makes this site feel the way it does? Typography weight? Whitespace? Animation energy? Card density?

### 1b. Catalog design patterns

Build an inventory before touching code:

| Dimension | What to note |
|-----------|-------------|
| Layout | Full-width vs. constrained, grid vs. flex, section count |
| Typography | Headline size/weight, body copy, font names (check `font-face` or network tab) |
| Colors | Background, text, accent, gradient usage |
| Spacing | Padding/margin rhythm that repeats across sections |
| Animations | Scroll-triggered fades, parallax, hover transforms, stagger timing |
| Images | Aspect ratios, overlay treatment, cropping style |
| Navigation | Sticky? Transparent-to-solid? Hamburger threshold? |
| Sections | List every section present top-to-bottom |

### 1c. Identify imagery needs

List every section needing a photo/illustration. Source from:
- **Unsplash**: `https://images.unsplash.com/photo-[ID]?w=1600&q=80` — always prefer this
- **Pexels**: direct URL from pexels.com
- **Picsum**: `https://picsum.photos/1600/900?random=N` — placeholder of last resort only

Match images to the **client's industry**, not the reference site's.

---

## Phase 2 — Build

**Output**: a single `index.html` (inline `<style>` and `<script>`) unless the site needs multiple pages — then create separate HTML files sharing a common nav/footer pattern.

### Brand application rules
- Replace ALL reference colors with the client's palette — no original colors survive
- Swap the site name everywhere: hero, nav, footer, `<title>`, `<meta description>`
- Use a placeholder SVG logo if no real logo is available — make it look intentional
- Keep the reference font choices if they're good; otherwise use Google Fonts equivalents

### CSS setup (always start with this)
```css
:root {
  --color-bg: #FFFFFF;
  --color-primary: #000000;
  --color-accent: #4297FF;
  --color-text: #333333;
  --font-heading: 'Space Grotesk', sans-serif;
  --font-body: 'Inter', sans-serif;
  --max-width: 1200px;
  --section-padding: 120px 40px;
}
```

### Animation pattern (Intersection Observer — always use this)
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(el => {
    if (el.isIntersecting) el.target.classList.add('visible');
  });
}, { threshold: 0.15 });
document.querySelectorAll('.animate').forEach(el => observer.observe(el));
```
```css
.animate { opacity: 0; transform: translateY(30px); transition: opacity 0.6s ease, transform 0.6s ease; }
.animate.visible { opacity: 1; transform: translateY(0); }
```

### HTML principles
- Semantic tags: `<section>`, `<article>`, `<nav>`, `<header>`, `<footer>`
- Mobile-responsive from the start — `@media` breakpoints, flexible grid
- `scroll-behavior: smooth` on `<html>`
- Vanilla JS only — no frameworks

---

## Phase 3 — Self-Eval (run before showing output to user)

After the first build, **screenshot your own output** and compare section-by-section to the reference screenshots.

Run this checklist internally:

- [ ] Hero heading size/weight matches reference hierarchy
- [ ] Section count matches reference (nothing missing)
- [ ] Section widths/paddings are proportional
- [ ] Font sizes in correct hierarchy
- [ ] All brand colors applied — no reference colors remain
- [ ] Images have correct aspect ratios and load
- [ ] Animations trigger at correct scroll position and feel smooth
- [ ] Navigation behavior matches (sticky, scroll effects)
- [ ] Footer is complete (links, contact, copyright)
- [ ] Mobile reflow is correct on narrow screens

Fix everything you find before presenting to the user.

Aim for **at least 2 screenshot-compare-fix cycles** before declaring done.

---

## Phase 4 — Pre-Launch Eval (mandatory gate)

Before telling the user the site is ready, invoke the **`site-eval` agent**:

```
Use the site-eval agent to evaluate [path/to/index.html] against [reference URL].
```

The eval agent will return a structured report. **Do not mark the site as complete until the eval agent returns a PASS verdict.** If it returns FAIL or CONDITIONAL:
- Address every blocking issue
- Re-run eval
- Only proceed to delivery after a PASS

---

## Phase 5 — Deliver

Before handing off:
- Remove any `console.log` statements
- Verify all CDN links (fonts, icons) are live
- Confirm all images load (no broken src)
- Add comment block at top:
  ```html
  <!-- Built with Static Site Replicator | Brand: [Name] | Reference: [URL] | Date: [YYYY-MM-DD] -->
  ```
- Save output to `outputs/[site-name]/index.html`

---

## Common Section Patterns

### Hero
```html
<section class="hero">
  <nav class="nav"><!-- logo + links --></nav>
  <div class="hero-content">
    <h1 class="animate">Big bold headline</h1>
    <p class="hero-sub animate">Supporting statement</p>
    <a href="#contact" class="btn btn-primary animate">CTA Button</a>
  </div>
</section>
```

### Listing/Cards Grid
```html
<section class="listings animate">
  <div class="grid">
    <article class="card animate">
      <img src="..." alt="..." loading="lazy">
      <div class="card-body">
        <h3>Title</h3>
        <p>Description</p>
        <a href="#" class="card-link">View →</a>
      </div>
    </article>
  </div>
</section>
```
