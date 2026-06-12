---
name: visual-generator
description: Generate a branded social visual for a LinkedIn or X post — an AI-generated background image (Hugging Face FLUX.1-schnell) with crisp SVG text/stats overlaid (charts, stat callouts, diagrams, quote). Falls back to a flat SVG when no image token is set. Use when the user wants a graphic, image, visual, infographic, or chart for a post. Keywords - visual, graphic, image, svg, infographic, chart, donut, diagram, quote card, social image, ai image, background.
allowed-tools: Read Write Bash
---

# Visual Generator

Produce an on-brand social visual: an **AI-generated background image** with a **crisp SVG text/data layer** on top. The image gives the post a real, scroll-stopping look; the text stays as SVG because image models can't render accurate letters or numbers. If no image token is configured, it gracefully falls back to a flat-color SVG — so it always produces something.

## Before anything: load the brand
Read `design-system.md` (this directory): colors, chart palette, typography, the **AI imagery** section (provider, model, style-prompt suffix, scrim), spacing, dimensions. Match it. If still a TODO template, use the documented defaults and flag them.

## Inputs
- The post text (LinkedIn and/or X), its **pull-quote / key takeaway**, and any numbers in it.
- The digest item / source (for accurate stats and attribution).
- An optional output directory (from the orchestrator); else write next to the post.

## Step 1: Pick the layout (per post)
This defines the SVG text/data layer. Prefer a data/diagram layout; use a quote layout only when there's nothing to visualize.
- **Stat** — 1–3 key figures → donut(s) / big-number callouts, each with a label + source.
- **Comparison / before–after** → two columns or paired bars.
- **Trend** → a simple line/bar chart (few points).
- **Process / levels / framework** → tier ladder, pyramid, numbered steps.
- **Quote** → feature the pull-quote large.

State which you chose, in one line. Never fabricate data — attribute every figure to its source.

## Step 2: Generate the AI background (Hugging Face FLUX.1-schnell)
Only if `HF_TOKEN` is set in the environment. Build a prompt that captures the post's *mood* (not its text) plus the design-system style suffix. Always include "no text, no words, no letters."

Example prompt: `"conceptual editorial illustration evoking clarity emerging from noise; <design-system style suffix>"`.

Generate and save `bg.png` into the output dir:

```bash
# requires: HF_TOKEN in env. Writes bg.png; prints OK or FAIL.
OUT="<output-dir>"; PROMPT="<your prompt here>"
gen() { curl -sS -f -X POST "$1" \
  -H "Authorization: Bearer $HF_TOKEN" \
  -H "Content-Type: application/json" -H "Accept: image/png" \
  -d "$(python3 -c 'import json,sys;print(json.dumps({"inputs":sys.argv[1],"parameters":{"width":1080,"height":1080}}))' "$PROMPT")" \
  --max-time 120 -o "$OUT/bg.png"; }
if [ -z "$HF_TOKEN" ]; then echo "NO_TOKEN"; \
elif gen "https://router.huggingface.co/hf-inference/models/black-forest-labs/FLUX.1-schnell"; then
  file "$OUT/bg.png" | grep -qi 'image' && echo "OK" || echo "FAIL"
else echo "FAIL"; fi
```

- If it prints `OK`, use the image background path below.
- If `NO_TOKEN` or `FAIL`, **skip the image** and use the flat-color fallback. Never block the pipeline on the image service. Common causes when the saved file is JSON rather than an image:
  - `403 ... sufficient permissions` → the `HF_TOKEN` lacks the **"Make calls to Inference Providers"** permission (use a fine-grained token with that box checked).
  - model still loading → retry once after ~20s.
  - provider doesn't serve this model → try another router provider path (e.g. `.../models/<model>?provider=fal-ai`) or another model.
- The legacy `api-inference.huggingface.co` host is retired (does not resolve); use the `router.huggingface.co` path only.

## Step 3: Composite — embed the image, scrim, then text
Build a **self-contained** `visual.svg` (1080×1080). Layer order, bottom to top:

1. **Background image** (if Step 2 succeeded): base64-encode and embed so the SVG needs no external files:
   ```bash
   B64=$(base64 -i "$OUT/bg.png" | tr -d '\n')
   # embed in SVG as: <image href="data:image/png;base64,${B64}" x="0" y="0"
   #   width="1080" height="1080" preserveAspectRatio="xMidYMid slice"/>
   ```
   If no image, use a flat `<rect fill="#0E1116">` (design-system Background) instead.
2. **Scrim** (only when an image is present): a full-canvas dark overlay + a bottom/left gradient for legibility, per the design-system AI-imagery scrim spec, e.g.:
   ```xml
   <rect width="1080" height="1080" fill="#0E1116" opacity="0.60"/>
   <defs><linearGradient id="g" x1="0" y1="1" x2="0" y2="0">
     <stop offset="0" stop-color="#0E1116" stop-opacity="0.85"/>
     <stop offset="0.55" stop-color="#0E1116" stop-opacity="0.2"/>
   </linearGradient></defs>
   <rect width="1080" height="1080" fill="url(#g)"/>
   ```
3. **Text/data layer** — the donuts, numbers, quote, title, neutral source footer from Step 1, exactly as the building blocks below. Keep the top accent bar. **No author name/handle** (standing preference); brand logo only if the design system defines one.

### Building blocks (reference)
- **Donut / ring** — two concentric `<circle fill="none">`: muted track + accent value ring with `stroke-linecap="round"`; `stroke-dasharray="<arc> <rest>"` where `arc = pct × (2πr)`; `transform="rotate(-90 cx cy)"`. `NN%` centered inside.
- **Bar chart** — `<rect>`s on a baseline; label values directly.
- **Tier ladder** — stacked `<rect>`s of increasing width/color intensity; label each.
- **Big-number callout** — large figure + short label + small source line.
- **Quote** — large pull-quote (2–4 wrapped lines).

Legibility over an image: keep text on the scrimmed area, use the design-system text colors (white / accent), and add a subtle text shadow only if needed.

## Step 4: Output & self-check
- Write `visual.svg` to the output dir.
- Validate XML: `python3 -c "import xml.dom.minidom as m; m.parse('<dir>/visual.svg')"`.
- Rasterize to PNG: `qlmanage -t -s 1080 -o <dir> <dir>/visual.svg` then rename `visual.svg.png` → `visual.png`. (WebKit renders the embedded base64 image.) If qlmanage doesn't produce a valid PNG, still ship `visual.svg` (opens in any browser) and say so.
- Clean up `bg.png` (it's embedded now) unless the user wants the raw image kept.
- Tell the user the paths; note the SVG is self-contained and the text is editable.

## Rules
- Text and numbers are ALWAYS SVG, never baked into the AI image.
- The AI prompt describes mood/abstract imagery only — always "no text, no words."
- `HF_TOKEN` is read from the environment; never hardcode or commit it (the repo is public).
- Graceful fallback to flat SVG whenever the token is missing or generation fails.
- Never fabricate chart data; attribute all figures. No author name on the visual.
