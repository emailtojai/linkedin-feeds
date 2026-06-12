---
name: visual-generator
description: Generate a branded SVG infographic to accompany a LinkedIn or X post — charts, stat callouts, diagrams, tier/ladder visuals, or a quote card when there's no data. Use when the user wants a graphic, image, visual, infographic, or chart for a post. Keywords - visual, graphic, image, svg, infographic, chart, donut, diagram, quote card, social image.
allowed-tools: Read Write Bash
---

# Visual Generator

Produce a self-contained, on-brand **SVG infographic** that pairs with a post. Everything is built from shapes, text, and the post's real data — no external image service, so it's free, versionable, and editable. Default output is a genuine infographic, not a text card.

## Scope note (read once)
This skill builds **vector infographics** (charts, diagrams, stat layouts, quote cards). It does **not** generate photorealistic images or illustrations — that requires a generative image model behind an API key, which is intentionally out of scope for this plugin. If the user explicitly wants a real photo/illustration, tell them it needs an external image API + key and is a separate add-on.

## Before anything: load the brand
Read `design-system.md` (this skill's directory): colors (incl. chart palette), typography, logo/wordmark, attribution, spacing, dimensions. Match it exactly. If it's still a TODO template, proceed with the documented defaults and flag that they're placeholders.

## Inputs
- The post text (LinkedIn and/or X), its **pull-quote / key takeaway**, and any numbers in it.
- The digest item / source (for accurate stats and attribution).
- An optional output directory (from the orchestrator); else write next to the post.

## Step 1: Pick the infographic type (per post)
Choose from the content. Prefer a **data/diagram infographic**; fall back to a quote card only when there's genuinely nothing to visualize.

- **Stat infographic** — the post has 1–3 key percentages or figures → donut(s) and/or big-number callouts, each with a label and source.
- **Comparison / before–after** → two-column or paired bars.
- **Trend** → a simple line or bar chart (few points).
- **Process / levels / framework** → a tier ladder, pyramid, numbered steps, or flow (e.g. the Tier 1/2/3 ladder).
- **Quote card (fallback)** → only for pure opinion/story posts with no data: feature the pull-quote large.

State which type you chose and why, in one line. Never fabricate data — if the post has no real numbers, don't invent a chart; use a diagram or quote card instead. Attribute every figure to its source.

## Step 2: Build the SVG
Write a **standalone `.svg`**. Requirements:
- Square **1080×1080** by default (or the design system's size).
- Only design-system colors, fonts, logo. Inline everything — must render offline (no external font/image URLs; logo as inline shapes or a wordmark).
- Real `<text>` elements (crisp, selectable), wrapped to fit with comfortable margins; nothing overflows the canvas.
- Clear hierarchy: title → the data/diagram → labels/sources. Generous padding.
- **Never put the author's name or handle on the visual** (standing preference in `design-system.md`). Add a brand logo/wordmark only if the design system defines one (a brand mark, not a personal name); otherwise no attribution at all.
- Legible on a phone: large type, high contrast, a handful of data points max.

### Building blocks (reference)
- **Donut / ring** — two concentric `<circle fill="none">`: a track in a muted color, and a value ring in an accent color with `stroke-linecap="round"`. Set the value with `stroke-dasharray="<arc> <rest>"` where `arc = pct × (2πr)` and `rest = (2πr) − arc`; rotate `-90` around the center so it starts at the top. Put the `NN%` as centered text inside.
- **Bar chart** — `<rect>`s on a shared baseline; label values directly; accent color for the key series.
- **Tier ladder / pyramid** — stacked `<rect>`s with increasing width and color intensity to encode magnitude/autonomy; label each; add a small axis caption.
- **Big-number callout** — large weight figure + short label + small source line.
- **Quote card** — large pull-quote (2–4 wrapped lines). No author name; brand logo only if defined.

## Step 3: Output & self-check
- Write `visual.svg` to the output dir (orchestrator) or alongside the post.
- Validate it's well-formed: `python3 -c "import xml.dom.minidom as m; m.parse('<path>')"`.
- Optionally render a PNG preview to eyeball layout, then delete it:
  `qlmanage -t -s 600 -o <dir> <path>` (macOS) or `rsvg-convert` if available.
- Tell the user the path, that it opens in any browser, and that the SVG text is editable for tweaks.

## Rules
- Default to a real infographic; quote card is the fallback only.
- Never fabricate chart data; attribute all figures.
- Stay on-brand: no off-palette colors, no random fonts.
- No photoreal/AI images here (see Scope note).
