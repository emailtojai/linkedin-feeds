---
name: trend-scanner
description: Scan free Medium RSS feeds and the web for fresh, post-worthy material on the user's topics. Use when the user wants to research a topic, find trends, gather sources, or kick off content creation for LinkedIn or X. Keywords - trend scan, research, medium rss, feeds, sources, content ideas, digest, what to write about.
allowed-tools: Bash WebFetch WebSearch Read Write
---

# Trend Scanner

Find 3–5 pieces of fresh material worth writing about, and write them to a structured digest the writers can consume.

## Inputs

- A **topic** (from the user or the orchestrator), e.g. "AI governance".
- The configured feeds in `rss-sources.md` (read it from this skill's directory).
- An optional **output directory** passed by the orchestrator. If none is given, create `output/<YYYY-MM-DD>-<topic-slug>/` from the repo root (slug = lowercase, hyphenated topic).

## Process

### 1. Load sources
Read `rss-sources.md` in this skill's directory. It lists Medium authors, publications, tags, and extra web-search keywords. If it still contains only TODO placeholders, tell the user the scanner has no feeds yet and ask them to fill it in (or proceed web-search-only for this run).

### 2. Pull Medium RSS (free, no auth)
Medium exposes free RSS. For each configured source, `curl` the feed and read the `<item>` entries (title, link, pubDate, and the excerpt in `<description>`/`<content:encoded>`):

```!
curl -sL --max-time 20 -A "Mozilla/5.0" "https://medium.com/feed/@AUTHOR"
curl -sL --max-time 20 -A "Mozilla/5.0" "https://medium.com/feed/tag/TAG"
curl -sL --max-time 20 -A "Mozilla/5.0" "https://medium.com/feed/PUBLICATION"
```

Substitute the real handles/tags from `rss-sources.md`. Parse the returned XML directly — extract title, link, pubDate, and a clean text excerpt (strip HTML). Prefer items from roughly the last 2 weeks; note the date of each.

Notes:
- Paywalled Medium posts only return an excerpt in RSS. That is expected and fine — do not attempt to bypass the paywall. Use the excerpt; the user reads the full article themselves if they want.
- If a feed returns nothing or errors, skip it and move on.

### 3. Supplement with web search
Run `WebSearch` on the topic plus the keywords from `rss-sources.md` to catch material beyond Medium (news, blogs, announcements). Use `WebFetch` to read a promising result's actual content when the snippet is thin.

### 4. Rank and select
From everything gathered, pick the **3–5 strongest** items for *this topic*. Favor:
- Recency (last ~2 weeks).
- A concrete angle the author could have an opinion on (not generic explainers).
- Specifics: data, a launch, a contrarian claim, a real case.
Dedupe near-identical stories; keep the best source for each.

### 5. Write the digest
Write `digest.md` into the output directory using **exactly** this contract (the writers depend on it):

```markdown
# Digest: <topic> — <YYYY-MM-DD>

## 1. <Title>
- **Source:** <author / publication / site>
- **Link:** <url>
- **Date:** <pubdate or "n/a">
- **Excerpt:** <2–4 sentence clean excerpt, no HTML>
- **Why it matters:** <1–2 sentences on the angle worth a post — what's the take?>

## 2. <Title>
...
```

End by telling the user where the digest was written and giving a one-line summary of each item so they can pick one to write about.

## Output
- File: `<output-dir>/digest.md`
- A short summary in chat listing the 3–5 items.

Do not write posts here — that is the LinkedIn Writer and X Writer's job. This skill only researches and produces the digest.
