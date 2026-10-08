# headless-youtube

A fully automated channel of short vertical videos that give people hope. Type one idea, or type nothing,
and every day a 50-second video goes out: a prayer for someone's first week at a new job, a letter to the
person who is always the strong one, one verse for the week they cannot fix. Written by AI, checked by
AI, voiced, rendered and published by a script, free at first and paid only where the numbers say so.

Nothing is sold. The channel offers hope and prayer, and speaks to anyone getting through life, mental
health struggles, job stress, or an ordinary hard day.

## Status

Planning. The documents below are the deliverable of phase 0; no code exists yet. The owner's original
brief is kept verbatim in `docs/BRAIN_DUMP.md` and every design choice can be checked against it.

## Read in this order

| Document | What it answers |
|---|---|
| [`docs/PROJECT_PLAN.md`](docs/PROJECT_PLAN.md) | Vision, goals, principles, the four end-to-end concepts from $0 to about $100 a month, phases, risks |
| [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) | How an idea becomes a published video: stages, the job file, the visual ladder, captions, render, publish, intake, hosting |
| [`docs/VIDEO_CONCEPTS.md`](docs/VIDEO_CONCEPTS.md) | The five visual formats from brand card to AI video, what each costs, and when to climb |
| [`docs/CONTENT_STRATEGY.md`](docs/CONTENT_STRATEGY.md) | Six pillars, twelve series, script rules, scripture and quote rules, mental-health safety rules |
| [`docs/DISTRIBUTION.md`](docs/DISTRIBUTION.md) | YouTube, TikTok, Instagram and Facebook rules: quotas, audits, scheduling, AI disclosure, monetisation policy |
| [`docs/RESEARCH.md`](docs/RESEARCH.md) | Every tool, API, price and policy considered, with a link and the date it was checked |
| [`docs/DECISIONS.md`](docs/DECISIONS.md) | What has been decided, what is proposed, what is still open |
| [`docs/ROADMAP.md`](docs/ROADMAP.md) | Seven phases with task checklists and acceptance tests |
| [`docs/BRAIN_DUMP.md`](docs/BRAIN_DUMP.md) | The idea in the owner's words, the questions it leaves open, and the ideas that grew from it |
| [`docs/JOURNAL.md`](docs/JOURNAL.md) | The work journal, newest first |
| [`examples/`](examples/) | The job JSON schema, a validated sample script, and the three prompt templates |

## The pipeline in one line

intake → script (LLM) → gate (second LLM plus hard rules) → voice (TTS) → visuals (brand card, stock
clip, AI image or AI video) → captions (word-timed) → render (FFmpeg, 1080x1920, under 60 s) → metadata →
publish (YouTube first) → track (analytics back into the idea queue)

## Principles

- Write to one person, not an audience.
- Free first; pay per series, only when retention data says so.
- Degrade, never die: a provider outage lowers the visual tier, it never stops the daily post.
- Look things up, never make them up: scripture from a file, quotes from a verified list, prices from a
  dated link.
- Safe by construction: crisis resources, banned phrases and a second-model judge live in the pipeline.
- Windows-first: it runs on the owner's PC before it runs anywhere else.

## Working in this repo

See [`CLAUDE.md`](CLAUDE.md) for the journal rule, commit conventions, and the rule that no secret ever
enters the repository.
