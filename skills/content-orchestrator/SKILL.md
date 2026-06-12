---
name: run
description: Run the full LinkedIn & X content pipeline end to end — research a topic, write a LinkedIn post and an X post in the author's voice, generate a matching visual, and stage everything in a review folder. Use when the user wants to create content for a topic, "research and write about X", or run the whole content engine. Keywords - content engine, pipeline, research and write, linkedin and x, full run, weekly content.
argument-hint: "<topic>"
user-invocable: true
allowed-tools: Read Write Bash WebFetch WebSearch
---

# Content Orchestrator — `/content-engine:run "<topic>"`

Run the four skills in sequence and stage a complete review package. Skills don't call each other automatically — you (Claude) execute each step here, passing outputs forward via files on disk.

The topic is `$ARGUMENTS`. If empty, ask the user for a topic (or offer a default from `trend-scanner/rss-sources.md`).

## Steps

### 0. Set up the output folder
From the repo root, create `output/<YYYY-MM-DD>-<topic-slug>/` (slug = lowercase, hyphenated topic). Use today's date. All artifacts go here. Tell the user the path.

### 1. Research — Trend Scanner
Run the **trend-scanner** skill for the topic, writing `digest.md` into the output folder. Then present the 3–5 items to the user as a short numbered list and **ask which one to write about** (or let them say "you pick" — then choose the strongest and say why). Do not proceed until an item is chosen.

### 2. LinkedIn — LinkedIn Writer
Run the **linkedin-writer** skill on the chosen digest item. Produce the full Output Contract (3 hooks, 1 post, 1 spicier variant, 3 first-comment ideas, pull-quote). Write the recommended post to `output/.../linkedin.md`. Keep the pull-quote for the next steps.

### 3. X — X Writer
Run the **x-writer** skill on the same digest item plus the LinkedIn post and its pull-quote. Produce the X Output Contract. Write the result to `output/.../x.md`.

### 4. Visual — Visual Generator
Run the **visual-generator** skill with the post text and pull-quote. It picks quote-card vs data-chart and writes `output/.../visual.svg`.

### 5. Metadata + handoff
Write `output/.../metadata.json`:

```json
{
  "topic": "<topic>",
  "date": "<YYYY-MM-DD>",
  "chosen_source": { "title": "...", "link": "..." },
  "artifacts": ["digest.md", "linkedin.md", "x.md", "visual.svg"],
  "status": "draft"
}
```

Then summarize for the user: the folder path, what's in each file, and a reminder that **nothing is published** — they review, edit, and post manually. Offer to revise any piece.

## Notes
- Respect each skill's voice loading: if `voice-profile.md` is still a template, stop and offer the voice interview before writing.
- Never auto-post to LinkedIn or X. This pipeline only stages drafts.
- Keep the user in the loop at step 1 (which item) and at the end (review).
