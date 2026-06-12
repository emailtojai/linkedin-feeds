# Design System — TEMPLATE (fill in your brand)

The Visual Generator reads this to build on-brand SVGs. Replace the TODO/placeholder values with your real brand. Sensible defaults are provided so visuals still render before you customize — but they're placeholders, not your brand.

## Canvas
- Default size: **1080 × 1080** (square)
- Safe margin / padding: **80px** on all sides

## Colors
<!-- TODO: your real hex values. -->
- Background:   `#0E1116`   <!-- placeholder: near-black -->
- Surface/card: `#161B22`   <!-- placeholder -->
- Primary text: `#FFFFFF`   <!-- placeholder -->
- Secondary text: `#9BA3AF` <!-- placeholder: muted gray -->
- Accent:       `#3B82F6`   <!-- placeholder: blue — change to your brand accent -->
- Accent 2 (charts): `#22C55E` <!-- placeholder: green -->

### Chart palette
<!-- Used by infographics. TODO: align with your brand. -->
- Series / positive:  `#3B82F6`   <!-- accent -->
- Risk / negative:    `#F59E0B`   <!-- amber — for "bad" stats like decommission/failure rates -->
- Donut track (unfilled): `#1B2330`
- Tier ramp (low → high): `#1F2937` → `#2B4A6F` → `#3B82F6`

## Typography
<!-- TODO: your preferred fonts. Use web-safe stacks so the SVG renders offline. -->
- Headline / quote font stack: `"Georgia", "Times New Roman", serif`  <!-- placeholder -->
- Body / label font stack:     `"Helvetica Neue", Arial, sans-serif`  <!-- placeholder -->
- Quote size: ~56px, line-height ~1.25
- Attribution size: ~28px
- Chart label size: ~24px

## Attribution — IMPORTANT
- **Do NOT put the author's name (or handle) on any visual.** No "Jai Sharma", no "CTO, Corcentric", no personal wordmark. This is a standing preference.
- A brand **logo/wordmark** may be added later (a brand mark, not a personal name). Until one is defined here, visuals carry no attribution at all.

## Logo (optional, brand mark only)
<!-- Leave empty for now = no logo. If you later add a brand mark (NOT your name),
     provide an inline SVG path or a brand wordmark + color. -->
- Type: none (no logo on visuals for now)

## Layout rules
- Quote cards: quote centered/left-aligned per preference; large type; one logo; minimal decoration.
- Data charts: one chart, max ~6 data points; label values directly; accent color for the key series.
- High contrast, legible on a phone screen. No off-palette colors.
