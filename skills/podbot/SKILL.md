---
name: podbot
description: Use the `podbot` CLI to semantically search and explore a curated podcast index (tech/VC/business podcasts including All-In, a16z, 20VC, YC, TBPN, Dwarkesh, Acquired, Odd Lots, etc.). Use this skill when the user wants to research ideas, trends, opinions, or specific topics by mining podcast content — e.g. "find what podcasters are saying about X", "research business opportunities in Y", "what did guest Z say about W", "summarize the thesis on [topic] across podcasts". Returns chapter-level matches with match percentages, episode summaries, and full transcripts on demand.
---

# podbot — Podcast Index CLI

`podbot` is a CLI that provides semantic search and listing over a curated podcast index (tech/VC/business podcasts).

## Setup

Check that the CLI is installed before using it:

```bash
command -v podbot || npm install -g podbot
```

Requires Node.js 20+.

## Command structure

```
podbot feeds list|search
podbot episodes list|search
podbot transcript <feed-slug/episode-slug>
```

Pass `--help` to any level if unsure: `podbot episodes search --help`.

## Feeds

List all indexed podcasts. Useful to understand what's available before searching.

```bash
podbot feeds list                    # lists all feeds (default limit 50)
podbot feeds list --search "ai"      # title filter
podbot feeds search "venture capital strategy"   # semantic search across feeds
```

Output includes: slug, author, episode count, last episode date, description.

## Episodes

The primary tool. Two modes: `list` (filter by feed/title) and `search` (semantic).

```bash
# List episodes from a specific feed
podbot episodes list --feed all-in-podcast --limit 20

# Title search within a feed
podbot episodes list --feed tbpn --search "gemini"

# Semantic search across ALL feeds and ALL chapters
podbot episodes search "AI replacing human services full stack" --limit 10

# Filter by publish date
podbot episodes search "defense tech" --since 2025-06-01 --limit 8
```

**Semantic search is the real power tool.** It returns:
- Episode match % (overall episode relevance)
- Episode summary (2-3 paragraphs)
- Chapter list with timestamps
- **Matched chapters** with match % and chapter-level summaries — these are the actual passages that matched your query

Match percentages below ~25% are usually noise; focus on results above 40% for high-signal answers.

## Transcripts

```bash
podbot transcript all-in-podcast/future-of-defense-tech          # print markdown
podbot transcript all-in-podcast/future-of-defense-tech --json   # structured JSON
podbot transcript all-in-podcast/future-of-defense-tech -o out.md
```

Use transcripts sparingly — they're long. Prefer `episodes search` chapter summaries first, only pull the transcript when you need exact quotes or deeper context.

## JSON output

Every command supports `--json`. Pipe to `jq` for scripting:

```bash
podbot episodes search "healthcare admin AI" --json --limit 20 | jq '.[] | {slug, match}'
```

## Strategy for research tasks

1. **Start broad with parallel searches.** Fire 4-8 `podbot episodes search` calls in parallel with varied phrasings of the question. Semantic search rewards variation — "AI rollup fragmented industries" finds different hits than "boring business automation opportunity."
2. **Read the chapter summaries, not the transcripts.** The chapter-level AI summaries are dense and high-quality — they usually give you the argument, key numbers, and named entities without needing the transcript.
3. **Identify recurring names and companies.** When the same company/person surfaces in unrelated searches, that's signal. Then do a targeted search like `podbot episodes search "Monumental construction robot subcontractor"` to go deep.
4. **Use `--since` to filter stale content** when the user asks about current/recent state (e.g., `--since 2025-10-01`).
5. **Output budget.** Large semantic searches return 30-40KB. Prefer `--limit 5-8` per query and run more queries in parallel rather than one huge query.
6. **Cite findings precisely.** When synthesizing, reference the feed slug and episode slug (e.g., `lightcone-podcast/hand-place-computer`) so the user can verify — this is the equivalent of `file:line` for podcast research.

## Common query patterns

| User intent | Good query |
|---|---|
| Business opportunities | `"underserved market gap problem no one is solving"` |
| Trend cascade | `"consumer trends mass market wellness biohacking"` |
| Specific industry | `"healthcare administrative burden paperwork insurance"` |
| Founder/person views | `"[person name] on [topic]"` |
| Investment thesis | `"[sector] venture capital thesis 2025"` |
| Counter-narrative | `"skeptical [trend] bubble bear case"` |

## Scope

The index is heavy on tech, VC, startups, and macro/business. Use `podbot feeds list` to see what's currently indexed. The index is NOT comprehensive for non-tech topics (sports, politics, culture, science outside AI/bio). For those, say so rather than synthesizing from thin signal.
