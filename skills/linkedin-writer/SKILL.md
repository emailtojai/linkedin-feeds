---
name: linkedin-writer
description: Write a strong, human-sounding long-form LinkedIn post in the author's voice. Use when the user wants a LinkedIn post, thought-leadership content, a founder/B2B post, a case study, a contrarian take, or a build-in-public update. Works from a raw idea or from a Trend Scanner digest item. Keywords - linkedin, thought leadership, personal brand, founder posts, b2b content, storytelling, case study, hooks, engagement, authority.
allowed-tools: Read Write
---

# LinkedIn Writer

Create LinkedIn posts that feel human, useful, and credible. Combines proven hook frameworks, clear narrative structures, proof-first writing, and practical CTA design — written in the **author's** voice, never generic.

## Before anything: load the voice

1. Read `voice-profile.md` (this skill's directory) — the author's audience, tone, vocabulary, themes, emoji/hashtag stance. Match it.
2. Read `human-voice-rules.md` (this skill's directory) — the anti-AI-tells guardrail. Obey it. This is what keeps the post from sounding like a chatbot.

If `voice-profile.md` is still a TODO template, tell the user the voice isn't set up yet and offer to run the voice interview first.

## Inputs (dual mode)

- **Raw idea:** the user hands you a topic/thought directly.
- **Digest item:** the orchestrator (or user) points you at an item in a Trend Scanner `digest.md`. When working from a source, ground proof in that article and keep its link for citation/first comment. Do **not** present the source's results as the author's own unless confirmed.

Length target: **150–500 words.**

## Phase 1: Audience + Positioning (CRITICAL)

Before writing, define:
- **Audience** — who is this for? (founders, recruiters, engineers, operators, marketers, sales leaders…)
- **Goal** — reach, authority, leads, hiring, trust, replies, profile visits?
- **Core insight** — the one thing worth remembering.
- **Proof** — what makes it believable (numbers, before/after, constraints, mistakes, outcomes)?

If proof is missing, use placeholders like `[X%]`, `[Y hours]` and request the exact values. Never invent them.

## Phase 2: Structure + Hook Selection

Pick a format first, then write. Formats: **Story · Framework · Contrarian · Case study · Teardown · Build-in-public.**

Draft 2–3 candidate hooks, then finalize one. Hook formulas:

- **Contrarian:** "Most people do [X]. That's exactly why they stay stuck." / "Unpopular opinion: [industry belief] is outdated."
- **Specific result:** "In [timeframe], we improved [metric] by [number]. Here's how." / "We cut [cost/time] by [X%]. Not with a new tool — by changing one workflow."
- **Mistake:** "I made this [role]-mistake for months. It cost us [outcome]." / "We shipped the wrong thing."
- **Framework:** "The [3-step] framework I use for [outcome]." / "If I had to restart as [role], I'd follow these 5 rules."
- **Question:** "Would you let your team do [X] without [Y]?" / "What's your biggest bottleneck in [domain] right now?"

## High-performing post formats

1. **"This runs now" (operational story)** — bold claim → "Not as a demo. As an actual [workflow]." → "Here's what it does:" 4–6 concrete bullets → measurable result → perspective line + CTA question.
2. **Case study** — starting problem → constraints → intervention → before/after metrics → key lesson → optional "comment TEMPLATE" CTA.
3. **Contrarian opinion** — challenge the popular view → why it fails in practice → 3 practical principles → a polarizing but constructive question.
4. **Framework post** — name the framework → 3–5 steps → one mistake to avoid → one "do this today" action.
5. **Build-in-public update** — what broke → what changed next → ask for informed feedback.

## Phase 3: Draft + Polish

- Hook in the first 1–2 lines (stops the scroll).
- 1–2 sentence paragraphs; easy to skim on mobile.
- Front-load specifics; cut generic filler.
- Plain language over hype. Emojis sparingly, only as section markers if the voice profile allows them.
- End with **one** clear CTA that matches the goal (don't ask for leads on a pure thought-leadership post).
- Hashtags: **3–6 max**, a niche + function + audience mix; no spammy broad-only tags.
- Keep claims realistic. Never invent outcomes, clients, or credentials.

## Persuasion principles

Specificity ("saved 3.2 hours/week" > "saved time") · Mechanism (explain *how*, not just outcome) · Credibility (mention tradeoffs, not only wins) · Relevance (tie to the audience's reality) · Clarity (one post = one idea).

## Output Contract (default)

Produce, in this order:
1. **3 hook options**
2. **1 full post** (the recommended one)
3. **1 spicier variant**
4. **3 first-comment ideas** (to deepen discussion; include the source link here if from a digest)
5. **Pull-quote / key takeaway** — one punchy line the Visual Generator and X Writer can reuse.

Optional on request: NL / EN / NL-EN variants · short + long versions · carousel text outline.

When run inside the orchestrator, also write the post to `<output-dir>/linkedin.md`.

## Quick prompt template

When the user gives a raw idea, fill: Audience / Goal / Topic / Proof points / Tone / CTA preference — then generate the Output Contract.

## Execution checklist (before finalizing)

- [ ] Hook is strong and specific
- [ ] Core claim is clear; one idea only
- [ ] At least one real proof signal (or flagged placeholder)
- [ ] Easy to skim on mobile
- [ ] CTA invites real conversation and matches the goal
- [ ] Hashtags relevant and limited (3–6)
- [ ] Tone matches `voice-profile.md`
- [ ] Trips none of the tells in `human-voice-rules.md`

## Common pitfalls

Generic "AI changed everything" with no concrete example · no proof signals · too many ideas in one post · CTA mismatch · over-formatting with noisy symbols.
