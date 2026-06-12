# LinkedIn & X Content Engine

A Claude Code plugin that automates research, writing, and visuals for LinkedIn and X by composing five specialized skills. You provide topics and sources; the plugin scans free Medium RSS feeds and the web for fresh material, drafts a LinkedIn post in **your** voice, drafts a separate X post, generates a matching SVG visual, and stages everything in a folder for your review.

**Nothing is auto-published.** No paywall bypass. No paid APIs required.

## The five skills

| Skill | What it does | Invoke |
|---|---|---|
| **trend-scanner** | Pulls Medium RSS + web search → `digest.md` of 3–5 post-worthy items | `/content-engine:trend-scanner` |
| **linkedin-writer** | Long-form LinkedIn post in your voice (150–500 words) | `/content-engine:linkedin-writer` |
| **x-writer** | Compressed, thread-friendly X post | `/content-engine:x-writer` |
| **visual-generator** | Branded SVG — quote card or data chart, chosen per post | `/content-engine:visual-generator` |
| **run** (orchestrator) | Runs all four in sequence into a dated review folder | `/content-engine:run "<topic>"` |

Each skill is self-contained and can be run on its own; `run` ties them together.

## Install

From a Claude Code session:

```
/plugin marketplace add /Users/jaisharma/Projects/linkedin-feeds
/plugin install content-engine
```

Then confirm the five skills appear (e.g. `/content-engine:run`).

## Configure (fill these in)

Four bundled reference files are templates with `TODO` placeholders. Fill them in before serious use:

1. `skills/trend-scanner/rss-sources.md` — your 3 topics, Medium authors/tags/publications, extra search keywords.
2. `skills/linkedin-writer/voice-profile.md` — your voice. **Don't hand-write this** unless you want to; instead ask Claude to "run the voice interview" and it'll build the profile from ~8 questions.
3. `skills/visual-generator/design-system.md` — brand colors (hex), fonts, logo/wordmark, attribution handle.
4. `skills/x-writer/x-guidelines.md` — already sensible; only touch if you have X Premium / long-form needs.

The anti-AI guardrail (`skills/linkedin-writer/human-voice-rules.md`) is shared by both writers and keeps output sounding human, not generated. It's ready to use.

## AI image setup (optional)

The Visual Generator can place an **AI-generated background image** (Hugging Face, FLUX.1-schnell) behind the crisp SVG text/stats. It's optional — without a token the engine falls back to a flat-color SVG and still works.

To enable it:
1. Create a **free** Hugging Face token (no credit card) at https://huggingface.co/settings/tokens. Choose **Fine-grained** and enable the **"Make calls to Inference Providers"** permission (a plain Read token is not enough).
2. Make it available as an env var, e.g. add to your shell profile (`~/.zshrc`):
   ```
   export HF_TOKEN=hf_xxxxxxxxxxxxxxxxx
   ```
   (or add it to Claude Code's settings `env`). Re-open your shell/session so it's loaded.
3. Run a visual — the background image appears automatically; text and numbers stay sharp because they're SVG, not baked into the image.

**Security:** the token is read only from `HF_TOKEN`. Never hardcode it or commit it — this repository is public. Free HF inference credits reset monthly; if you run out, the engine simply falls back to flat SVG.

## Use it

Interactive (one command runs the pipeline):

```
/content-engine:run "AI governance"
```

This creates `output/<date>-ai-governance/` containing:
- `digest.md` — the research
- `linkedin.md` — the LinkedIn post (+ hooks, variant, first-comment ideas)
- `x.md` — the X post/thread
- `visual.svg` — the graphic, self-contained (open in a browser to preview); `visual.png` when rendered. With `HF_TOKEN` set, it has an AI-generated background.
- `metadata.json` — topic, sources, `status: draft`

You review, edit, and post manually.

Or run skills individually, e.g. just research, or just rewrite an idea for X.

## How it fits together

The skills pass data through files. The Trend Scanner writes a `digest.md` in a fixed format; the writers consume a chosen item from it; the visual generator reads the post's pull-quote. The orchestrator's `SKILL.md` drives the sequence — there's no programmatic skill-to-skill call, just instructions Claude follows.

## Later: scheduling (optional, deferred)

For unattended weekly content, a cloud **routine** can run `/content-engine:run` on a cron (~1-hour minimum) and write the review package to a folder/branch for you to review asynchronously. Not set up yet — interactive mode first.

## Design decisions

- **Medium RSS is free** and officially supported; paywalled posts return excerpts only, which is all we use.
- **Visuals are SVG** by code — versionable, editable, no image API.
- **Voice profile is the foundation** — spend time on it; it's what makes output sound like you.
- **Always human-in-the-loop** before anything is published.
