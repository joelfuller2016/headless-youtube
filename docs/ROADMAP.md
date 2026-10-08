# Roadmap

Phases are ordered so that each one ships something that runs on its own. Checkboxes are the task
list; tick them in the pull request that does the work and journal the result. "Done when" is the
acceptance test and is not negotiable by the person doing the work.

## Phase 0 — Plan (this pull request)

- [x] Capture the owner's idea verbatim (`docs/BRAIN_DUMP.md`)
- [x] Research tools, prices, APIs and policies with links and dates (`docs/RESEARCH.md`)
- [x] Design the pipeline (`docs/ARCHITECTURE.md`) and the format ladder (`docs/VIDEO_CONCEPTS.md`)
- [x] Write the content and safety rules (`docs/CONTENT_STRATEGY.md`)
- [x] Write the job schema, a sample, and the three prompts (`examples/`)
- [x] Record decisions and open questions (`docs/DECISIONS.md`)
- [ ] Owner reviews the plan and accepts or changes D-001 to D-019

**Done when:** the plan is merged and the owner has answered the open questions in `docs/BRAIN_DUMP.md`.

## Phase 1 — Walking skeleton, $0, local only

- [ ] `src/hy` package with a `run` command and the stage loop from `docs/ARCHITECTURE.md`
- [ ] Intake from the CLI (`hy new "idea"`) writing `output/<id>/job.json`
- [ ] Script stage with the generator prompt and schema validation (one retry)
- [ ] Gate stage in the researched order: schema and stop reason, word count and hook length, banned-phrase regex, attribution allowlist, distinctness score against the last 30 scripts, profanity, free moderation call, judge on a second model, audio duration
- [ ] Voice stage with the chosen local TTS and a fallback provider
- [ ] Visual stage, rung A only: twelve brand backgrounds, three colour themes
- [ ] Caption stage producing an ASS file with word timing and emphasis colouring
- [ ] Render stage with FFmpeg: bumper, scenes, captions, music bed, loudness normalisation, thumbnail
- [ ] Dry-run flag that stops before publish, on by default
- [ ] `tests/`: schema test, golden render of `examples/sample-script.json`

**Done when:** `hy new` followed by `hy run` turns the sample idea into a 50 to 60 second MP4 on the
owner's Windows PC in under two minutes, with no network call except the LLM.

## Phase 2 — YouTube, unattended

- [ ] Google Cloud project, YouTube Data API enabled, OAuth consent screen set to **In production**
- [ ] One-time OAuth flow (loopback redirect, user account; service accounts do not work) that stores the refresh token outside the repo
- [ ] One real API upload before the publish stage is built, to see whether the private-until-audit rule still applies; record the result in `docs/DECISIONS.md`
- [ ] Publish stage for YouTube: private upload, `publishAt`, `selfDeclaredMadeForKids=false`,
      `containsSyntheticMedia` from the render tier, title and description rules
- [ ] Submit the YouTube API Audit and Quota Extension form
- [ ] Windows Task Scheduler job running `hy run` hourly as System (not with a saved password); a `PAUSE` file honoured
- [ ] Daily summary to Telegram or email; failure alert with the stage and error
- [ ] `review_mode=notify` for the first two weeks

**Done when:** seven consecutive days of automatic uploads with no manual step except, until the audit
passes, flipping the video to public.
- [ ] Comment safety on heavy videos: YouTube hold-for-review with the crisis keyword list, the resource block as the pinned first comment, a daily owner sweep; the bot never replies to a crisis comment

## Phase 3 — The content system

- [ ] Six pillars and the series catalogue from `docs/CONTENT_STRATEGY.md` as configuration
- [ ] Self-feeding idea generator with the rotation calendar and the last-30-ideas memory
- [ ] Crisis-resource block appended automatically for high-sensitivity topics
- [ ] Heavy-topic gate: keyword-checked classifier, safe-messaging lint, help-seeking close, `review_mode: approve` for that video (D-019)
- [ ] Read section 8 of Orygen's #chatsafe guidelines (US edition) by hand and paraphrase the influencer rules into the writer prompt; the PDF is copyrighted and too large to fetch
- [ ] Confirm NIV terms in a browser before any NIV use; ESV and NIV stay off until then (D-006)
- [ ] Scripture lookup from a public-domain translation file so references are never invented
- [ ] An eval set of 20 to 30 scripts with human pass or fail labels, re-run against the judge whenever a prompt, model or threshold changes
- [ ] GitHub issue form as the one idea queue; Telegram polling and self-feed open issues into it
- [ ] Brand kit: fonts, colours, bumper, end card, licensed music bed with its licence file in `assets/`
- [ ] Telegram (or GitHub issue form) intake so ideas can be sent from a phone
- [ ] GitHub Actions workflow in a private repository as the scheduler of record: cron at an odd minute, `workflow_dispatch` with idea and publish inputs, FFmpeg install step, state committed back, 25-minute job timeout, Discord or Telegram report; the PC becomes the backup

**Done when:** thirty days unattended with no more than two failed jobs and zero rejected-for-safety
videos published.

## Phase 4 — Rung B and the feedback loop

- [ ] Stock adapter for Pexels (portrait video search, cache by term, allow-list per mood) and Pixabay
- [ ] Downgrade ladder B → A exercised by a test
- [ ] Track stage: YouTube Analytics pull on days 1, 3, 7, 28 into `output/metrics.csv`
- [ ] Series scoreboard in the daily summary; the idea generator reads the top series

**Done when:** every pillar has at least five published videos on rung B and the scoreboard shows
retention per series.

## Phase 5 — More platforms

- [x] Decide D-010 from the comparison in `docs/RESEARCH.md` (direct Meta and Bluesky adapters; Buffer for TikTok)
- [ ] Public HTTPS object storage behind a domain the owner controls, with a short-lived copy per publish
- [ ] Meta app with the Instagram Login flavour (professional account, Standard Access), the owner's Facebook Page, and the Threads tester role
- [ ] Adapters: Instagram Reels, Facebook Reels, Threads, Bluesky; distribution ledger with daily counters, token refresh and remote ids
- [ ] Buffer free plan connected for TikTok, posting one a day through its API; upload-post as the paid alternative
- [ ] Per-platform metadata rules from `docs/DISTRIBUTION.md`
- [ ] Budget guard live, cap $25 a month
- [ ] Phase 5b: Pinterest Trial then Standard (demo video, Business account); LinkedIn share product

**Done when:** one upload fans out to at least three platforms automatically for fourteen days.

## Phase 6 — Paid quality rungs

- [ ] Image adapter (rung C) with prompt caching and a fixed style
- [ ] Hybrid (rung E) for the top two series by retention
- [ ] Video adapter (rung D) for hook scenes only, behind the budget guard, cap $100 a month
- [ ] Disclosure flag set automatically from the tier

**Done when:** a 30-day comparison shows whether the paid rungs beat rung B on retention per dollar.

## Phase 7 — Hardening

- [ ] Backup of `output/*/job.json` and `metrics.csv` to the repo or object storage
- [ ] Modal or Hugging Face Jobs function for the GPU steps, capped at Modal's free credit
- [ ] Runbook: what to do when a token expires, a provider changes its price, or a platform changes a rule
- [ ] Quarterly re-check of every dated claim in `docs/RESEARCH.md` and `docs/DISTRIBUTION.md`

**Done when:** the channel survives a full week with the owner's PC switched off.
