# `assets/` — README + social media imagery

Visual artifacts for the README and social channels.

| File | Purpose | Used by |
|---|---|---|
| `demo.gif` | Hero animation cycling through 4 real screens of Persona Studio (the product `/cto` built) | README top |
| `hero.png` | Single static landing screenshot of Persona Studio | GitHub repo Settings → Social preview |
| `make_demo_gif.py` | Earlier iteration: PIL-rendered synthesized terminal animation. Kept for reference. | manual |
| `capture_persona.py` | Playwright script that captured the real Persona Studio screenshots and stitched them into `demo.gif` | regeneration |

## Regenerating the GIF

```bash
python3 assets/capture_persona.py
```

Requires Playwright (`pip install playwright && playwright install chromium`). Edit the URL or screen-capture sequence inside `capture_persona.py` to point at a different demo product.

## Social-preview convention

GitHub renders `assets/hero.png` as the OpenGraph image when the repo is shared on X, LinkedIn, Slack, Discord. Keep it 1280×800 or wider, with the headline + product visible without zooming.
