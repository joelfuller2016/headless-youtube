# Journal

One file, newest entry first, UTC timestamps. Every instruction and every piece of work gets a line.
Numbers and identifiers come from commands run at the time; a correction is a new entry naming what it corrects.

## Standing instructions from the owner

- Journal every piece of work here and push the journal straight to `main`; everything else goes through a PR.
- Research before deciding and link every resource used.
- Free or cheap first; design for expansion.
- Offer several concepts for a 100 percent automated process, not one.

## Log

<!-- journal: new entries go directly below this line -->
- 2026-10-08 11:10Z On branch `claude/headless-youtube-automation-qjoh25`: commits `35b2bbb` (brain dump, architecture, schema, prompts), `46152c8` (project plan, distribution, format ladder, content strategy, decisions, roadmap), `f45cc3f` (corrections: FFmpeg is not on the GitHub Ubuntu runner image; YouTube Audio Library music is only guaranteed claim-free on YouTube; Analytics API scopes). Facts in those docs were read from official pages on this date: YouTube Data API gives `videos.insert` its own bucket of 100 calls/day at 1 unit each; unaudited projects' uploads are forced private; `publishAt` needs `privacyStatus=private`; `status.containsSyntheticMedia` exists; Shorts up to 3 min since 2024-10-15 and claimed Shorts over 1 min are blocked; YPP 1,000 subs + 10M Shorts views/90 days (expanded tier 500 subs + 3M); inauthentic-content rename on 2025-07-15; Testing-status OAuth refresh tokens expire in 7 days; TikTok unaudited clients post private only; Instagram 100 API posts/24 h with a public video URL; Pexels 200 req/h and 20,000/month; GitHub public-repo scheduled workflows disable after 60 idle days; ESV gratis use up to 500 verses with notice; KJV public domain outside the UK; WEB and BSB public domain; 988 call/text/chat 24/7. Research workflow `wf_4e8cbbee-4ce`: 2 of 9 dimensions complete at 11:06Z.
- 2026-10-08 10:49Z Bootstrapped the empty GitHub repo: pushed `main` with commit `075c269` (`.gitignore`, placeholder README) so a PR can be opened against it. Created branch `claude/headless-youtube-automation-qjoh25` from it.
- 2026-10-08 10:48Z Launched a 19-agent research workflow (9 dimensions, each fact-checked by a second agent, plus a completeness critic) covering open-source pipelines, TTS, visuals, captions and music, YouTube API and policy, other platforms and schedulers, hosting, LLM scripting and intake, content strategy and safety.
- 2026-10-08 10:45Z Received the project brief from Joel (kept verbatim in `docs/BRAIN_DUMP.md`). Checked the sibling repos for earlier notes on this idea: none found. The `headless-youtube` remote had no commits.
