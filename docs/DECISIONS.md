# Decisions

One entry per decision, newest at the bottom, never deleted. A reversed decision gets a new entry that
names the one it replaces. **Accepted** means the plan is built on it; **Proposed** means it is the
recommendation and the owner has not yet said yes; **Open** means the options are listed and a
recommendation is pending evidence.

| ID | Decision | Status | Date |
|---|---|---|---|
| D-001 | Reference implementation in Python 3.12; FFmpeg for rendering | Proposed | 2026-10-08 |
| D-002 | One JSON job file per video is the only state; no database in v1 | Proposed | 2026-10-08 |
| D-003 | Captions rendered from an ASS file burned in by FFmpeg; Remotion optional later | Proposed | 2026-10-08 |
| D-004 | Videos are capped at 60 seconds by default | Proposed | 2026-10-08 |
| D-005 | YouTube first, through the Data API; submit the compliance audit in phase 2 | Proposed | 2026-10-08 |
| D-006 | Scripture only from public-domain translations; licensed translations only within their gratis limits and with the required notice | Proposed | 2026-10-08 |
| D-007 | Phase 1 voice: Kokoro-82M locally as primary, Google Cloud TTS free tier as fallback; `edge-tts` for prototyping only | Proposed | 2026-10-08 |
| D-008 | Script model and judge model are different models | Proposed | 2026-10-08 |
| D-009 | Phases 1 and 2 run on the owner's Windows PC from Task Scheduler as the development runner; from phase 3 the scheduler of record is a GitHub Actions workflow in a private repository; GPU steps go to Modal's free credit or Hugging Face Jobs | Proposed | 2026-10-08 |
| D-010 | Phase 5 publishes directly to Instagram Reels, Facebook Reels, Threads and Bluesky at $0; TikTok uses its inbox Upload route at $0 (the owner finishes each post with one tap) until the budget allows upload-post Basic at $24 a month, because Buffer's API cannot create TikTok posts; Pinterest and LinkedIn after their paperwork; X skipped | Proposed | 2026-10-08 |
| D-011 | `review_mode` defaults to `none`; `notify` for the first two weeks | Proposed | 2026-10-08 |
| D-012 | Budget caps: $0 for phases 1 to 4, up to $25 a month in phase 5, up to $100 a month in phase 6 | Proposed | 2026-10-08 |
| D-013 | No voice cloning of the owner in v1 | Proposed | 2026-10-08 |
| D-014 | Monetisation is not a goal; the content rules still comply with YouTube's monetisation policies | Proposed | 2026-10-08 |
| D-015 | Visual rungs A and B launch together; C, D and E are switched on per series by data | Proposed | 2026-10-08 |
| D-016 | HeyGen (AI presenter) and ChatCut (agent-driven editor) are deferred: HeyGen is a phase-6 optional rung F after a one-series test; ChatCut is used interactively for prototyping and reconsidered as a rung-D provider once its automation terms are confirmed | Proposed | 2026-10-08 |
| D-017 | MiroFish (swarm-simulation prediction engine) is not part of the pipeline; it may be tried after phase 4 as an audience rehearsal for ranking series concepts, under the budget guard | Proposed | 2026-10-08 |
| D-018 | Hugging Face assets adopted: ACE-Step 1.5 as the $0 generated music bed, Qwen3-TTS in the voice bake-off, Z-Image-Turbo as the second rung-C model, Wan2.1-T2V-1.3B as the small-GPU local video model, ZeroGPU Spaces and Inference Providers as the GPU-free hosted path, public-domain scripture parquet files for the lookup stage | Proposed | 2026-10-08 |
| D-019 | Heavy-topic videos (suicide, self-harm, eating disorders, abuse, addiction, acute grief) run through the safe-messaging lint, carry the crisis block in the description and in the first comment (pinned by the owner at approval), and wait for the owner's approval even when the channel runs unattended; the approve timeout can never publish one | Proposed | 2026-10-08 |

## D-001 Python and FFmpeg

**Why.** Every speech, image, video and platform SDK ships a Python client first; FFmpeg is the one
renderer that runs the same on Windows, a Pi and a CI runner. The owner's .NET skills are not wasted: a
.NET port using FFMpegCore and the Google YouTube client is viable for rungs A and B, and the job JSON
contract makes a language swap possible stage by stage. **Reversible:** yes, per stage.

## D-002 Job files, no database

**Why.** One folder per video with a `job.json` that validates against `examples/script-schema.json` is
enough state for years at one video a day, survives any crash, diffs cleanly, and needs no server.
**Revisit when:** more than a few thousand jobs, or more than one machine writes at once.

## D-003 ASS captions through FFmpeg

**Why.** Deterministic styling, no Node toolchain, works on Windows with a bundled font. Remotion is the
upgrade for animated captions and is free for individuals and companies with up to three employees under
its licence. **Reversible:** yes; captions are a separate stage.

## D-004 Sixty-second cap

**Why.** YouTube blocks any Short over one minute that carries an active Content ID claim, so staying
under 60 seconds removes a whole class of silent failure. Sixty seconds also keeps the same file eligible
as a short-form post on every other platform. The cap is configuration; a series can raise it
deliberately. Source and date in `docs/DISTRIBUTION.md`.

## D-005 YouTube first, Data API, audit early

**Why.** YouTube is the stated channel and its API is the most workable of the big platforms for one
developer. The trap is that uploads from an unaudited API project are forced private; so phase 2 submits
the Audit and Quota Extension form as soon as the first private upload works, and the runner uploads as
private with a `publishAt` either way. Fallback until the audit passes: the owner flips videos public by
hand, which is a one-tap job. Sources in `docs/DISTRIBUTION.md`.

## D-006 Public-domain scripture

**Why.** The King James Version is public domain outside the United Kingdom; the World English Bible and
the Berean Standard Bible are public domain by their publishers' statements. A licensed translation such
as the ESV may be quoted without written permission up to 500 verses with its full copyright notice and,
in audio and video, a spoken credit. That notice does not fit in a 55-second video, so the default is
public domain, and the script generator is told so. Sources in `docs/CONTENT_STRATEGY.md`.

## D-007 Phase 1 voice

**Why.** Kokoro-82M's weights are Apache-2.0, it runs on a Windows CPU, and the Kokoro-FastAPI wrapper
exposes an OpenAI-compatible endpoint that also returns caption timestamps, which removes the forced
alignment step. Google Cloud Text-to-Speech gives one million characters a month free on its current
voices (a billing account must exist), which covers more than a thousand scripts, so it is the
zero-cost cloud fallback. `edge-tts` works and is free, but it reaches Microsoft's voices through an
unofficial route that Microsoft staff have said may breach their terms for commercial use, and it
breaks when the Edge endpoint changes, so it is kept for prototyping only. Several open models were
ruled out because their weights forbid commercial use (XTTS-v2, F5-TTS, Fish Speech), and two hosted
services are gone or going (PlayHT, Hume). Paid voices with emotion control (Cartesia Pro at $5 and
ElevenLabs Creator at $22, both priced in the section 10 cost sheets, with `gpt-4o-mini-tts` as a
bake-off candidate) are the phase-6 upgrade; ElevenLabs' free plan is
non-commercial and requires a credit in the title, so it is never used. Sources and prices with dates
in `docs/RESEARCH.md`. **Reversible:** yes, the voice provider is an interface.

## D-008 Two models, and a $0 fallback chain

**Why.** A judge grading its own writer's output is a weak gate, and LLM judges have documented position,
verbosity and self-enhancement biases, so the judge is a different model family grading one script
against a rubric with a constrained verdict. Cost is not the constraint: Claude Haiku 5.5 writes a script
for about $0.0007 and Opus 5.5 for about $0.03, so a month costs under $1 on Opus at one video a day and
about $3 at three a day with a `gpt-5-mini` judge; a free chain (Gemini Flash free tier, OpenRouter free models, a local Ollama model) sits behind
the same client interface so a quota error never stops the daily post. Prices and sources in
`docs/RESEARCH.md` section 8.

## D-009 Windows PC first, GitHub Actions as the scheduler of record from phase 3

**Why.** $0, no deployment, the owner can watch it work, so the PC is the right place to build and the
right manual backup. It is a poor production scheduler: a Task Scheduler task with a saved password
stops silently when the password changes, and sleep, hibernate and update reboots skip runs. GitHub
Actions costs nothing for one run a day (2,000 free minutes a month on a private repository), has
secrets, a cron trigger, a dispatch trigger with inputs and an issues trigger, and a job may run six
hours; FFmpeg is installed each run and there is no GPU, so GPU steps go to Modal (free $30 a month of
credit, per-second billing, its own cron) or Hugging Face Jobs. In a public repository the schedule
switches off after 60 idle days; a private repository avoids that and keeps prompts out of public view.
Sources and dates in `docs/RESEARCH.md` section 7. **Reversible:** yes; the runner is the same code on
every host.

## D-010 Direct APIs where they are free, an aggregator only for TikTok

**Why.** The comparison in `docs/RESEARCH.md` section 6 showed that Instagram Reels (through the
Instagram-Login flavour, which needs no Facebook Page and no App Review for the owner's own account),
Facebook Reels, Threads (tester role) and Bluesky can all be published to by a personal app for nothing
and with no review, provided the MP4 sits at a public URL. TikTok cannot: unaudited apps post privately,
and TikTok's guidelines name personal upload utilities as unacceptable for the audit, so a self-hosted
scheduler with the owner's own TikTok app would stay private too. Buffer's free plan was the planned $0
bridge until the verification pass read Buffer's developer guides on 2026-10-08: the API creates posts
for ten platforms and TikTok is not one of them, so a Buffer TikTok channel can only be fed by hand in
the app. The $0 route is therefore TikTok's own inbox Upload (no audit; the video lands in the owner's
TikTok inbox and one tap publishes it), and upload-post Basic ($24 a month, TikTok included with no
audit) is the fully automatic one once the phase-5 budget allows it. Corrected the same day; the
earlier wording is superseded, not deleted. Pinterest and LinkedIn wait for their paperwork, X is skipped.
**Reversible:** yes; adapters are independent.

## D-011 Review mode

**Why.** The brief asks for 100 percent automation, so `none` is the default. For the first two weeks
`notify` sends every rendered video to the owner before it posts, with no wait, so mistakes are seen
the same day.

## D-012 Budget caps

**Why.** Free first, expand later, in the owner's words. Caps are enforced by the budget guard in the
runner, not by discipline.

## D-013 No voice cloning in v1

**Why.** A consistent licensed AI voice is simpler, avoids any likeness question, and YouTube treats
cloning one's own voice as a minor edit anyway, so the option stays open.

## D-014 Comply with monetisation policy regardless

**Why.** YouTube's inauthentic content policy targets mass-produced, repetitive content. Complying is the
same thing as making the channel worth watching, so it costs nothing extra and keeps the door open.

## D-015 Launch on rungs A and B

**Why.** Both are free, both are robust, and together they cover every pillar. Paid rungs are turned on
per series once there are 30 days of retention data to justify them. See `docs/VIDEO_CONCEPTS.md`.

## D-016 HeyGen and ChatCut

**Why.** Both were raised by the owner on 2026-10-08 and researched the same day (`docs/RESEARCH.md`
section 11). HeyGen puts a realistic synthetic presenter on screen, which is a different format from the
faceless brief, costs roughly $1 to $4 a minute through its pay-as-you-go API on third-party figures that
could not be confirmed on an official page, has no free API credits since February 2026, forbids
commercial use of free-plan output, and always requires YouTube's synthetic-media disclosure. ChatCut is
priced sensibly for AI video and voice generation and ships a Claude Code plugin and a CLI, but its terms
prohibit automated use, the plugin only runs inside desktop agent hosts, and its edits are
non-deterministic. Neither blocks anything; both are kept as options with the conditions stated.
**Reversible:** yes.

## D-017 MiroFish

**Why.** Raised by the owner on 2026-10-08 and read the same day (`docs/RESEARCH.md` section 11). It
predicts how a simulated population reacts to seed material; it produces no video, voice or post. A run
is thousands of LLM calls plus a cloud memory service, its interface is interactive, and the phase-4
analytics loop measures the real audience for free. It stays an optional experiment for ranking series
concepts, never a gate on a daily video. **Reversible:** yes.

## D-018 Hugging Face assets

**Why.** A survey of the Hub on 2026-10-08 (`docs/RESEARCH.md` section 12) found four things the earlier
research had missed or ruled out: a licence-clean local music generator (ACE-Step 1.5, MIT, under 4 GB of
VRAM, Windows package), an Apache-licensed voice with instruction-driven emotion (Qwen3-TTS), an
Apache-licensed 6B image model with text rendering that fits a 16 GB card (Z-Image-Turbo), and a
1.3B Apache-licensed video model that runs in 8 GB (Wan2.1-T2V-1.3B). The platform itself gives a free
account five GPU minutes a day on shared Spaces and lets it host two Spaces, and one token reaches the
same hosted providers section 3 priced. Each is an option behind an existing interface, not a new stage.
**Reversible:** yes.

## D-019 Heavy-topic videos wait for the owner

**Why.** The platforms allow recovery and encouragement content and remove anything that promotes or
instructs self-harm, and the gap between the two is wording a lint can catch most of the time but not
always. The 988 press guidance asks for a referral number, a safe-commenting policy and the number in
the first comment; the Recommendations for Reporting on Suicide name the phrases to avoid. Those become
code. What code cannot judge is whether a particular script, on a particular day, is the one that should
not go out, so the one class of video where a mistake can hurt someone gets a human look, and the bot
never answers a crisis comment. Two mechanics follow: the approve-mode timeout, which may publish an
ordinary job by config, can only fail a heavy-topic job; and because the Data API cannot pin a comment,
the pipeline posts the crisis block as the first comment and the owner pins it at approval. Evidence in
`docs/RESEARCH.md` section 9; rules in `docs/CONTENT_STRATEGY.md` section 6. **Reversible:** yes, by changing `review_mode` for the heavy
template, though the default should not change without a reason written here.
