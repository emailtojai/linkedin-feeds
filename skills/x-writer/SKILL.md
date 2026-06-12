---
name: x-writer
description: Write a compressed, scroll-stopping X (Twitter) post or thread in the author's voice, adapted for X's pace and constraints. Use when the user wants an X post, a tweet, a thread, or an X adaptation of a LinkedIn post or research item. Keywords - x, twitter, tweet, thread, hot take, viral, short-form, hook.
allowed-tools: Read Write
---

# X Writer

Write for X: faster, punchier, and more opinionated than LinkedIn. One sharp idea, a hook that survives the first line, no corporate polish.

## Before anything: load the voice

1. Read `../linkedin-writer/voice-profile.md` — the canonical voice profile (shared across both writers). Match it.
2. Read `../linkedin-writer/human-voice-rules.md` — the anti-AI-tells guardrail. Obey it.
3. Read `x-guidelines.md` (this directory) — X-specific limits and rules.

If the voice profile is still a TODO template, tell the user to run the voice interview first.

## Inputs (dual mode)

- **Raw idea / hot take** from the user, or
- **A Trend Scanner digest item**, or
- **A finished LinkedIn post + its pull-quote** (from the orchestrator) to adapt for X.

When adapting from LinkedIn: don't just truncate. Re-cut for X — lead with the spiciest line, drop the throat-clearing, keep one idea.

## How X differs from LinkedIn

- **Shorter and faster.** No "Phase 1 / Phase 2" build-up. The first line is the whole battle.
- **More opinionated.** X rewards a clear stance and a little edge. LinkedIn diplomacy reads flat here.
- **Less formatting.** No hashtag blocks, no emoji section markers. At most 0–2 hashtags, often zero.
- **Thread only when needed.** If the idea fits one post, keep it one post. Don't pad to a thread.

## Process

1. **Pick the angle** — one idea, one stance. Sharper than the LinkedIn version.
2. **Write the hook (the first post).** It must stand alone and make someone stop. Strong patterns:
   - Blunt claim: "Most X advice is wrong. Here's what actually works:"
   - Specific result: "Cut our [metric] [X%] in [time]. The fix was embarrassingly simple:"
   - Contrarian: "Unpopular: [belief] is a trap."
   - Curiosity gap: "I spent [time] doing [X] so you don't have to. 5 things:"
3. **Decide single vs thread** (see `x-guidelines.md`). Default to a single post unless the idea genuinely needs steps/list/story beats.
4. **If a thread:** hook tweet → one idea per tweet, each able to stand semi-alone → final tweet with the takeaway + a light CTA (a question or "follow for more like this," matched to voice). Optionally end with the source link.
5. **Tighten.** Every tweet under the limit. Cut filler words. Read it aloud (per the guardrail).

## Output Contract (default)

1. **2–3 hook options** for the first post.
2. **The recommended post or thread**, tweets numbered (`1/`, `2/`, …) if threaded, each with an approximate character count.
3. **1 spicier variant** of the hook.
4. Note any source link to attach (usually in the last tweet or a reply).

When run inside the orchestrator, also write the result to `<output-dir>/x.md`.

## Checklist

- [ ] Hook works as a standalone first line
- [ ] One idea, clear stance
- [ ] Every tweet within the character limit (`x-guidelines.md`)
- [ ] Thread used only because the idea needs it
- [ ] 0–2 hashtags, no spam
- [ ] Sounds like the author (voice profile) and trips no AI tells
