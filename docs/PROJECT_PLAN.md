# Project plan — a fully automated channel of short videos that give people hope

**Status:** draft for the owner's review, 2026-10-08. Companion documents: `docs/ARCHITECTURE.md` (how the
pipeline works), `docs/VIDEO_CONCEPTS.md` (the format ladder), `docs/CONTENT_STRATEGY.md` (what it says
and what it must never say), `docs/DISTRIBUTION.md` (platform rules), `docs/RESEARCH.md` (every tool and
price with a link and a date), `docs/DECISIONS.md`, `docs/ROADMAP.md`. The owner's original brief is in
`docs/BRAIN_DUMP.md`.

## 1. Vision

Type one idea, or type nothing at all, and every day a short vertical video goes out that speaks to one
tired person: a prayer for their first week at a new job, a letter to the one who is always the strong
one, one verse for the week they cannot fix. Made by AI, checked by AI, posted by a script, paid for with
pocket change, and built so it keeps running when the owner is busy living his own life.

## 2. Goals and non-goals

**Goals**

- 100 percent automated from idea to published post, with an optional one-tap approval mode.
- A true $0 tier that produces watchable videos, and a path to spend money only where it visibly helps.
- Content that is specific, kind, safe around mental health, honest about scripture, and never generic.
- Creation and distribution both solved; YouTube first, more platforms after.
- Every tool, price and rule documented with a link and the date it was checked.

**Non-goals**

- Selling anything. No products, no courses, no affiliate links.
- A web dashboard, a database, comment management, or voice cloning in version one.
- Chasing monetisation. The channel complies with monetisation rules because that is the same as being
  good, not because it needs the money.

## 3. Principles

1. **Write to one person.** Every script names who it is for and the moment they are in. This is the
   craft rule and the policy rule at once.
2. **Free first, then pay per series.** Rungs A and B cost nothing; rungs C to E are switched on per
   series by retention data, never by taste.
3. **Degrade, never die.** A provider outage lowers the visual rung; it never stops the daily post.
4. **Look things up, never make them up.** Scripture comes from a file, quotes from a verified list,
   prices from a dated link.
5. **Safe by construction.** Crisis resources, banned phrases and a second-model judge are in the
   pipeline, not in anyone's memory.
6. **Idempotent and resumable.** One job file per video, every stage safe to rerun, no state anywhere else.
7. **Windows-first.** It runs on the owner's PC before it runs anywhere else.

## 4. The system in one paragraph

An idea enters from the CLI, a GitHub issue, a Telegram message, a spreadsheet row, or the system's own
daily generator. A language model writes a script as structured JSON: hook, scenes, close, visual
prompts, hashtags, and a safety block. A second model judges it against the content rules and either
passes it, sends it back once, or rejects it. A text-to-speech engine reads it. Each scene gets a visual
from the lowest rung that works: a brand card, a stock clip, an AI image, or an AI video clip. Word-timed
captions are generated, FFmpeg composes everything into a 1080x1920 video under 60 seconds with a music
bed, and the publish stage uploads it to YouTube with a scheduled time and, later, fans it out to other
platforms. Days later the tracker pulls the numbers back so the idea generator learns which series
deserve the next dollar. Details: `docs/ARCHITECTURE.md`.

## 5. Four concepts for a 100 percent automated process

All four run the same pipeline and share the same job file; they differ in where the runner lives and
which providers sit behind each stage. Costs assume one video a day and are explained line by line, with
links and dates, in `docs/RESEARCH.md`. The budget caps are decision D-012.

### Concept 1 — "Zero dollar, home PC"

| Stage | Provider | Cost |
|---|---|---|
| Intake | CLI and the self-feeding generator; Telegram bot later | $0 |
| Script and judge | a free-tier hosted model (Gemini Flash free tier or an OpenRouter free model) for the script, a local Ollama model for the judge, or the other way round | $0 |
| Voice | Kokoro-82M locally on CPU (Apache-2.0); Google Cloud Text-to-Speech free tier as fallback | $0 |
| Visuals | rung A brand cards; rung B stock from Pexels and Pixabay | $0 |
| Captions | word timings from the TTS, or forced alignment with faster-whisper on CPU | $0 |
| Render | FFmpeg with ASS captions | $0 |
| Publish | YouTube Data API, private upload with `publishAt` | $0 |
| Tracking | YouTube Analytics API | $0 |
| Hosting | Windows Task Scheduler running `hy run` hourly on the owner's PC | $0 (electricity) |

**Monthly cost:** $0. **Catch:** the PC must be on for the runner to fire; the first videos stay private
until YouTube's API audit passes; free-tier model limits must be watched. **Best for:** phases 1 to 4.

### Concept 2 — "Serverless on GitHub Actions"

Same providers as Concept 1, hosted differently: a public repo with a scheduled workflow runs the
pipeline on a GitHub-hosted Ubuntu runner (FFmpeg is not on the standard image, so the workflow installs it
with one `apt-get` step, about 30 seconds), ideas arrive
as GitHub issues using an issue form, results and metrics are committed back, and the rendered MP4 is
attached to a release so other platforms can fetch it from a public URL. Secrets live in the Actions
secret store.

**Monthly cost:** $0 on a public repo (standard runners are free for public repositories; a private repo
gets 2,000 free minutes a month on the Free plan). **Catch:** no GPU, so local TTS runs on CPU and AI
images are API calls or a Hugging Face ZeroGPU Space inside the free five minutes a day; scheduled workflows in a public repo switch off after 60 days without
repository activity, so the journal commits matter; artifact storage is 500 MB on the Free plan, so
media go to releases. **Best for:** phase 7 as the second host, or phase 1 if the owner's PC is
unreliable.

### Concept 3 — "Low-code with n8n"

A self-hosted n8n instance (Docker Desktop on the PC, or a $5-a-month VPS) runs the same stages as
nodes: a schedule trigger, an HTTP node to the model, a TTS node, a render step calling either a local
FFmpeg script or a hosted render API (Creatomate, Shotstack, JSON2Video), and a publish node. Many
community templates exist for exactly this shape, and the visual canvas makes failures easy to see.

**Monthly cost:** $0 to $5 for hosting, plus whatever the render API charges if one is used. **Catch:**
community templates usually assume paid services; the self-hosted licence allows personal use but
should be read; two systems to maintain if the Python stages are still used for rendering. **Best for:**
the owner who wants to see the pipeline rather than read logs.

### Concept 4 — "Managed quality stack"

The same pipeline with paid providers behind the expensive stages: a premium TTS voice, AI images on
every scene and an AI video clip on the hook, and a paid aggregator that posts to YouTube, TikTok,
Instagram, Facebook and Pinterest from one upload and handles their app audits (upload-post at $24 a
month or Blotato at $29). Hosted on a small VPS so the PC is out of the loop.

**Monthly cost:** roughly $50 to $120 at one video a day, dominated by the aggregator and AI video; the
exact line items and their dates are in `docs/RESEARCH.md`. **Catch:** every paid provider is a
dependency that can change its price or its terms; the downgrade ladder is what makes that survivable.
**Best for:** phase 6, and only for the series that earned it.

### Which one, and when

Start with Concept 1, move the runner to Concept 2 as a second host when it has proven itself, and
borrow Concept 4's paid providers one stage at a time, per series, behind the budget guard. Concept 3 is
an alternative front end for the same stages rather than a different destination.

## 6. The format ladder, in short

| Rung | On screen | Cost per video | When |
|---|---|---|---|
| A brand card | branded background, animated captions | $0 | launch |
| B stock loop | calm real-world clip per scene, captions | $0 | launch |
| C AI image | generated image per scene with motion | cents | after 30 videos, top series |
| D AI video | generated 5 to 8 s clips | $0.50 to several dollars | hooks only, after data |
| E hybrid | D or C for the hook, B elsewhere, A for cards | pennies to dimes | steady state |

Full treatment, including the three formats that cost nothing extra (kinetic typography, the letter
format, the scripture card), in `docs/VIDEO_CONCEPTS.md`.

## 7. Content, in short

Six pillars (hope, prayer, mental health, job and career, encouragement, happiness) on a daily rotation,
twelve repeatable series, 130 to 160 spoken words a video, written to one specific person, scripture
looked up from public-domain translations, named quotes only from a verified file, a banned-phrase list
the judge cannot be talked out of, and a crisis-resource block appended automatically on heavy topics.
Full rules in `docs/CONTENT_STRATEGY.md`.

## 8. Distribution, in short

YouTube Shorts first through the Data API: 100 uploads a day in the default quota, scheduled with
`publishAt`, the made-for-kids and synthetic-media flags set per video, and the compliance audit
submitted early because unaudited projects' uploads are forced private. Phase 5 adds Instagram Reels,
Facebook Reels, Threads and Bluesky through their own APIs at no cost and with no review for the owner's
own accounts, with the MP4 served from public object storage behind the owner's domain; TikTok goes
through Buffer's free plan or a paid aggregator because its audit does not accept personal upload
tools; Pinterest and LinkedIn follow their paperwork (decision D-010). Facts, quotas and sources in
`docs/DISTRIBUTION.md`.

## 9. Phases

| Phase | Ships | Done when |
|---|---|---|
| 0 Plan | this document set | owner has reviewed and answered the open questions |
| 1 Walking skeleton | idea → MP4 on the PC, rung A, $0 | sample renders in under two minutes, no publish |
| 2 YouTube unattended | scheduled private uploads, audit submitted, alerts | seven consecutive automatic days |
| 3 Content system | pillars, series, self-feed, scripture file, brand kit, phone intake | thirty days unattended, two failures or fewer |
| 4 Rung B and feedback | stock adapters, analytics pull, series scoreboard | five rung-B videos per pillar, retention per series visible |
| 5 More platforms | aggregator or direct APIs, public hosting, budget guard at $25 | one upload reaches three platforms for fourteen days |
| 6 Paid rungs | AI image, hybrid, AI video for hooks, cap $100 | a thirty-day comparison of retention per dollar |
| 7 Hardening | backups, second host, runbook, quarterly fact re-check | a week with the PC switched off |

Task-level checklists in `docs/ROADMAP.md`.

## 10. Risks and the cheapest mitigation for each

| Risk | Likelihood | Mitigation |
|---|---|---|
| Videos stay private because the YouTube API audit is slow or refused | high at first | upload private with `publishAt`; owner flips to public by hand until the audit passes; submit the form in phase 2, week one |
| The OAuth refresh token expires after seven days | certain if the app is left in Testing | set the consent screen to In production before the first unattended run |
| A free TTS or model endpoint changes or disappears | medium over a year | every stage has a fallback provider; the runner alerts on fallback use |
| The channel trips the Spam policy's mass-production rule (strikes) or reads as inauthentic (demonetised) | medium, and the naive pipeline is the policy's own example | the variety rules in `docs/CONTENT_STRATEGY.md` section 11: no shared music bed or visual set between videos, rotating structures, narrative in every video, the owner's own perspective layer, one to three videos a day, provenance logged |
| A fabricated or misattributed quote is published | medium without controls, low with them | scripture from a file, quotes from a verified list, judge rejects unverifiable quotes |
| Harmful wording on a mental-health topic | low with controls | banned-phrase scanner, sensitivity levels, crisis block, YouTube's own guidance followed |
| Music triggers a Content ID claim and blocks a video | low | YouTube Audio Library or a clearly licensed bed; videos under 60 seconds |
| Stock clip repeats make the channel look like every other | medium | cache by term, allow-list per mood, rotate sources, climb to rung C for top series |
| The PC is off and nothing posts | medium | `publishAt` schedules a day ahead; GitHub Actions as the second host in phase 7 |
| Spend creeps up once paid rungs are on | medium | budget guard with daily and monthly caps, downgrade on cap, spend in the daily summary |

## 11. Open questions for the owner

Listed with the assumption used so far in `docs/BRAIN_DUMP.md`, section 3. The two that most change the
build: which platforms beyond YouTube matter in the first three months, and whether the first two weeks
should run in `notify` or `approve` review mode.

## 12. Resources

Every tool, API, price and policy referenced in this plan is listed with its link and the date it was
checked in `docs/RESEARCH.md`. Policy and quota sources are repeated next to the facts they support in
`docs/DISTRIBUTION.md` and `docs/CONTENT_STRATEGY.md`.
