# Research — tools, prices, APIs and policies, with links and dates

Every price, quota and licence term below was read on the linked page on the date in the section
heading unless a line says otherwise. Prices change; the roadmap's phase 7 re-checks this file
quarterly. "Impression" marks a quality judgement that was not measured. Where two sources disagreed,
both are shown.

How this was gathered: nine research passes, one per section, each followed by a second, adversarial
pass that re-fetched the numeric and policy claims and tried to refute them; a final pass looked for gaps.
The project owner's own checks of the YouTube, TikTok, Instagram, Pexels, GitHub and scripture facts are
cited inline in `docs/DISTRIBUTION.md` and `docs/CONTENT_STRATEGY.md`.

Contents

1. Open-source pipelines and render engines
2. Text-to-speech
3. Visuals: stock, AI images, AI video
4. Captions and music
5. YouTube API and policy (summary; detail in `docs/DISTRIBUTION.md`)
6. Other platforms and schedulers
7. Orchestration and hosting
8. Script generation, quality gates and idea intake
9. Content strategy and safety (market notes; rules in `docs/CONTENT_STRATEGY.md`)
10. Cost sheets for the four concepts

---

## 1. Open-source pipelines and render engines (checked 2026-10-08)

**What the field looks like.** Every "idea to vertical short" project that is alive in 2026 converges on
the same free, Windows-friendly recipe: a language model writes the script, a free text-to-speech voice
reads it, Pexels or Pixabay or a local image set supplies visuals, word timings come from the voice
engine or `faster-whisper`, captions are burned in from an ASS subtitle file through FFmpeg's libass, and
the result is uploaded with the YouTube Data API. Almost all of them stop at an MP4; distribution is the
weak link everywhere. The recommendation that follows from this is to own the pipeline (it is a few
hundred lines around FFmpeg) and use the best reference projects for ideas.

### End-to-end generators

| Project | Licence | Activity | Windows | Headless | Uploads | Notes |
|---|---|---|---|---|---|---|
| [MoneyPrinterTurbo](https://github.com/harry0703/MoneyPrinterTurbo) (harry0703) | MIT | very active: pushed 2026-10-08, release v1.3.8 on 3 Oct, 129,314 stars | yes, one-click `.7z` package with `start.bat` | REST API on `:8080` and `cli.py --batch-file` (up to 100 tasks) | only through the paid Upload-Post service (`upload_post_*` keys), not platform APIs | 9:16, 16:9, 1:1; Edge TTS default or `faster-whisper` captions; Pexels, Pixabay, Coverr; optional paid AI video providers; Python 3.11+, 4 to 8 GB RAM |
| [purffle-shorts](https://github.com/Chamanrajragu/purffle-shorts) | MIT | pushed 2026-09-30, 27 stars | yes, documented | CLI `python -m purffle_shorts` plus an MCP server | YouTube Data API v3 with OAuth and scheduling, `YT_DAILY_LIMIT` default 6 | FFmpeg only (no MoviePy, no ImageMagick, no GPU); six word-synced caption styles; a second-LLM "script doctor" pass. **Best small reference for this project's shape.** |
| [youtube-shorts-pipeline](https://github.com/rushindrasinha/youtube-shorts-pipeline) | MIT | pushed 2026-06-09, 2,312 stars | not documented | CLI | YouTube OAuth, private by default, with SRT and thumbnail | stills with Ken Burns motion, Whisper word-level ASS captions; README quotes about $0.04 a video on the budget path and $0.11 premium |
| [tube-assistant](https://github.com/metiutek/tube-assistant) | MIT | pushed 2026-09-12, 27 stars | yes, `.bat` helpers | Telegram-controlled agent | YouTube Data API v3 | claims $0 a month on free tiers; optional paid AI b-roll capped by a setting; a good pattern for phone control |
| [ShortGPT](https://github.com/RayVentura/ShortGPT) | MIT | stale: last push 2025-02-10 | Docker or Colab only | Gradio UI only, no documented API | no | read for ideas, do not depend on |
| [MoneyPrinter v1](https://github.com/FujiwaraChoki/MoneyPrinter) | MIT | pushed 2026-03-26, PRs closed | ImageMagick needed | web UI and queue | not described | voices come from unofficial third-party TikTok-TTS proxies with no auth: fragile and a terms risk |
| [MoneyPrinterV2](https://github.com/FujiwaraChoki/MoneyPrinterV2) | AGPL-3.0 | pushed 2026-09-15, 32,065 stars | partly documented | CLI with cron | undocumented | AGPL only matters if the pipeline is offered as a service |
| [short-video-maker](https://github.com/gyoridavid/short-video-maker) | MIT (renders with Remotion) | dormant since 2025-06-21 | **not supported** per its README | REST and MCP | no | Kokoro voice, whisper.cpp captions, Pexels only, English only |
| [ViralMint](https://github.com/openclaw-easy/ViralMint) | AGPL-3.0 | pushed 2026-09-30 | yes | browser UI plus REST | no | FFmpeg with ASS captions, local motion-graphics engine |
| [openshorts (jnMetaCode)](https://github.com/jnMetaCode/openshorts) | MIT | pushed 2026-10-01 | partly | CLI and MCP | no (exports a publish package) | zero-key default path with Wikimedia Commons media; installs its own libass FFmpeg |
| [auto-shorts](https://github.com/alamshafil/auto-shorts) | MIT | **archived** 2024-12 | | | | do not adopt |
| [Zvid daily-quote-videos](https://github.com/Zvid-io/daily-quote-videos) | MIT sample over a paid API | pushed 2026-09-23 | n8n | n8n | no, ends at a public MP4 URL | the exact "sheet of quotes, one video a day" pattern, worth copying with an owned renderer |

Two otherwise useful 2026 repos (`Dark2C/Viral-Faceless-Shorts-Generator`, `aredwan-xyz/video-autopilot`)
ship no licence file, so their code cannot be reused; `video-autopilot` is still worth reading for its
GitHub Actions `daily.yml`.

### Render engines

| Engine | Licence | Activity | Windows | Notes |
|---|---|---|---|---|
| [FFmpeg](https://ffmpeg.org/download.html) with libass | LGPL 2.1+, GPL when built with libx264 (the gyan.dev Windows builds are GPLv3 static) | 9.0.2 released 2026-09-18 | `winget install "FFmpeg (Essentials Build)"`, `choco`, `scoop` | the renderer for rungs A to E; ASS karaoke tags give word highlights; call it as a subprocess, not through the dormant `ffmpeg-python` (last push 2024-08) or `editly` (2025-05) |
| [Remotion](https://github.com/remotion-dev/remotion) | its own licence: free for individuals, companies up to 3 employees, non-profits ([LICENSE.md](https://github.com/remotion-dev/remotion/blob/main/LICENSE.md)); Company License $25 a seat a month or $0.01 a render with a $100 a month minimum ([remotion.pro](https://www.remotion.pro/license)) | very active: 62,469 stars, pushed 2026-10-08 | yes, x64; pass `--props` as a file, inline JSON breaks in Windows shells | the upgrade for animated captions; official TikTok template installs whisper.cpp; Lambda example prices a 1-minute render at about $0.017 to $0.021 of AWS compute plus S3 ([cost example](https://www.remotion.dev/docs/lambda/cost-example)); the pending 5.0 licence counts contractors toward headcount |
| [Revideo](https://github.com/midrender/revideo) | MIT | 0.11.0 on 2026-07-10, 4,090 stars | not documented | licence-clean alternative with headless `renderVideo()` |
| [MoviePy](https://github.com/Zulko/moviepy) | MIT | 2.2.1 on 2025-05-21 | yes | v2 dropped ImageMagick and broke the v1 API most tutorials use; its own README says it is slower than FFmpeg directly |

### Caption renderers worth reading

[captacity](https://github.com/unconv/captacity) (stale since 2024-06) and
[ai-video-captions](https://github.com/nicolaigaina/ai-video-captions) (a one-day repo, 2026-03-27, six
styles via `pysubs2` and FFmpeg) encode the "word highlight" look; the ASS-karaoke pattern they use is easy
to own and is what `docs/ARCHITECTURE.md` specifies.

### Distribution services these projects lean on

[Upload-Post](https://www.upload-post.com/llms-full.txt): free tier 10 uploads a month across Instagram,
LinkedIn, YouTube, Facebook, X, Threads, Pinterest, Reddit and Bluesky but **not TikTok**; Basic $24 a
month ($16 a month billed annually) for unlimited uploads and all platforms including TikTok. Daily
posting exceeds the free tier. [Taisly](https://github.com/taisly/agent) (MIT client, hosted service, no
card to start, prices not listed) is the agent-first alternative. More in section 6.

### What this section decides

- Own the pipeline on FFmpeg (decision D-001, D-003). Crib from `purffle-shorts` and
  `youtube-shorts-pipeline`.
- If a batteries-included tool is ever wanted, MoneyPrinterTurbo is the only popular one that is both
  maintained and Windows-packaged; run it as a local service with `upload_post_enabled=false` and upload
  through the Data API directly.
- Remotion only for animated captions, locally, under the free licence; Revideo if headcount grows.
- Skip ShortGPT, MoneyPrinter v1 and V2, auto-shorts, captacity, editly, ffmpeg-python as dependencies.

## 2. Text-to-speech (checked 2026-10-08)

**Summary.** At 130 to 160 words a video (about 900 characters) and one to three videos a day, roughly
27,000 to 81,000 characters a month, voice is effectively free at every tier. What differs is licence
safety, naturalness, emotion control, and whether it runs on a Windows CPU.

| Option | Type | Price | Free allowance | Commercial use | Windows CPU | Notes |
|---|---|---|---|---|---|---|
| [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) via [Kokoro-FastAPI](https://github.com/remsky/Kokoro-FastAPI) or [kokoro-onnx](https://github.com/thewh1teagle/kokoro-onnx) | open weights, local | $0 | unlimited | yes, Apache-2.0 weights; the author notes it "has been deployed in commercial APIs" | yes: `pip install kokoro` plus the espeak-ng MSI; FastAPI ships `start-cpu.ps1` and a CPU Docker image; about 3.5 s first-token on an older i7 (batch is fine) | 54 voices, 8 languages (v1.0, 2025-01-27); FastAPI exposes an OpenAI-compatible endpoint with caption timestamps; no emotion control; `kokoro` pins Python below 3.13. **Phase 1 primary (D-007).** |
| [Google Cloud Text-to-Speech](https://cloud.google.com/text-to-speech/pricing) | cloud | Chirp 3 HD $30 per 1M chars after the free 1M a month; Neural2 $16; Standard and WaveNet $4 after 4M free | 1M chars a month on current voices, 4M on Standard; a billing account must exist and overage is charged | yes under standard cloud terms (service-specific terms not fetched) | yes, official SDKs | about 1,000 scripts a month inside the free allowance. **Phase 1 fallback (D-007).** |
| [Azure AI Speech](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/speech-services/) | cloud | Neural $15 per 1M chars, Neural HD $22 (read from the [Azure Retail Prices API](https://learn.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices) filtered to East US Speech products, because the pricing page renders prices in JavaScript) | 0.5M chars a month on F0; 20 requests a minute, no batch synthesis ([quotas](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-services-quotas-and-limits)) | yes | yes, first-class .NET SDK | the official, terms-clean way to the same voices `edge-tts` uses |
| [OpenAI `gpt-4o-mini-tts`](https://developers.openai.com/api/docs/pricing) | cloud | $0.60 per 1M input text tokens plus $12 per 1M audio output tokens; `tts-1` $15 per 1M chars; `tts-1-hd` $30 | none | yes; usage policy requires telling listeners the voice is AI-generated | API | natural-language style instructions ("warm, unhurried"); 2,000-token input cap; cents a month at this volume, exact per-minute cost not published |
| [ElevenLabs](https://elevenlabs.io/pricing) | cloud | Starter $6 a month list (30,000 credits, about 30 min), Creator $22 (121,000 credits); heavy promotions in October 2026, budget at list | Free plan 10,000 credits, **non-commercial only, and requires an "elevenlabs.io" credit in the title** ([help centre](https://elevenlabs.io/docs/help-center/legal/can-i-publish-the-content-i-generate-on-the-platform), [terms](https://elevenlabs.io/terms-of-use)) | paid plans only | API | the naturalness benchmark; Starter covers one video a day. Never use the free plan for a public channel. |
| [Cartesia](https://cartesia.ai/pricing) | cloud | Pro $5 a month, 100,000 credits (1 credit = 1 character; 750 to 800 a minute) | Free 20,000 credits a month, no commercial licence | Pro and above | API | budget alternative with a commercial licence |
| [Amazon Polly](https://aws.amazon.com/polly/pricing/) | cloud | Standard $4, Neural $16, Generative $30 per 1M chars | Standard 5M a month; Neural 1M, Generative 100K "for the first 12 months", which may not apply to accounts created under the newer credit-based free tier | yes ("your Polly output belongs to you") | SDKs | fine, but the free-tier rules are murkier than Google's |
| [Chatterbox](https://github.com/resemble-ai/chatterbox) (Resemble AI) | open weights, local | $0 | unlimited | yes, MIT | Nano (110M) runs about 3x realtime on 8 CPU cores per the README; Turbo and Multilingual want a GPU | emotion "exaggeration" control and tags like `[laugh]`; every file carries an inaudible PerTh watermark. The expressive open option if a GPU appears. |
| [Piper](https://github.com/OHF-Voice/piper1-gpl) | open weights, local | $0 | unlimited | yes (GPL-3.0 affects redistributing code, not audio) | yes, 1.8.0 (2026-09-04) ships a Windows wheel | fast, noticeably flatter than Kokoro (impression); fallback only |
| [edge-tts](https://github.com/rany2/edge-tts) | unofficial client for Microsoft Edge's online voices | $0 | unlimited in practice | **grey**: a Microsoft moderator wrote that commercial use without an Azure subscription "could be a violation of our terms of service" ([Microsoft Q&A, 2024-10-07](https://learn.microsoft.com/en-us/answers/questions/2088770/are-opensource-edge-tts-free-for-commercial-use)); a question about monetised YouTube went unanswered | yes | breaks when Microsoft rotates the endpoint (7.2.3 and 7.2.6 did; 403s in January 2026); writes SRT subtitles. **Prototyping only.** |
| XTTS-v2, F5-TTS, Fish Speech (local weights) | open weights | $0 | | **no**: Coqui Public Model Licence (non-commercial, and Coqui is gone), CC-BY-NC-4.0, Fish Audio Research Licence ("no commercial rights") | | excluded even though the channel sells nothing; a public channel is commercial in these licences' terms |
| Hume AI, PlayHT | cloud | | | | | **excluded**: Hume is sunsetting its TTS API with access ending 2026-11-13 ([changelog](https://dev.hume.ai/changelog)); PlayHT's sites were unreachable and third-party reports say it shut down on 2025-12-31 after Meta acquired the team |

**Decisions this section supports.** D-007: Kokoro locally as primary, Google Cloud TTS as the free cloud
fallback, `edge-tts` for prototyping only, OpenAI or ElevenLabs Starter as the phase-6 upgrade for emotion.
Before committing to a voice, run a ten-script listening test across Kokoro (`af_heart`, `am_michael`),
Chirp 3 HD, `gpt-4o-mini-tts` and ElevenLabs; arena rankings could not be fetched (tables render
client-side), so quality claims above are vendor claims or impressions.

**Open questions.** Audio tokens per minute for `gpt-4o-mini-tts` (not published); whether ElevenLabs'
with-timestamps endpoint is on Starter; Kokoro's real-time factor on a typical desktop CPU (measure in the
bake-off); whether Polly's 12-month allowances apply to new accounts.

## 5. YouTube API and policy (checked 2026-10-08)

The full treatment with design consequences is in `docs/DISTRIBUTION.md`; this is the fact sheet.

| Fact | Value | Source |
|---|---|---|
| Default Data API quota | 100 `search.list` calls, 100 `videos.insert` calls, and 10,000 units a day for everything else; the two named methods have their own buckets at 1 unit a call; resets at midnight Pacific | [Getting started](https://developers.google.com/youtube/v3/getting-started), [Quota costs](https://developers.google.com/youtube/v3/determine_quota_cost) |
| Stale figure still on the quota page | the summary box says `videos.insert` costs 1,600 points; the bucket text below it is the current rule | same |
| Unaudited projects | uploads via `videos.insert` from unverified projects created after 28 July 2020 are restricted to private; an audit lifts it | [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert) |
| Audit and more quota | the Audit and Quota Extension form; periodic audits; appeals form | [Quota and compliance audits](https://developers.google.com/youtube/v3/guides/quota_and_compliance_audits) |
| Scheduling | `status.publishAt` only with `privacyStatus=private` on a never-published video; a past time publishes immediately | [Videos resource](https://developers.google.com/youtube/v3/docs/videos) |
| Required flags | `status.selfDeclaredMadeForKids`; `status.containsSyntheticMedia` for realistic altered or synthetic content | same |
| Title | 100 characters, no `<` or `>` | same |
| File size | up to 256 GB | [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert) |
| AI disclosure | required for realistic likenesses, altered real footage, realistic scenes that did not occur, AI music as the main focus; not for scripts, captions, or cloning your own voice | [Disclosing altered or synthetic content](https://support.google.com/youtube/answer/14328491) |
| OAuth token life | External apps in Testing status get refresh tokens that expire in 7 days; 100 refresh tokens per account per client | [Using OAuth 2.0](https://developers.google.com/identity/protocols/oauth2) |
| Shorts definition | square or vertical, up to three minutes, uploaded on or after 15 October 2024 | [Three-minute Shorts](https://support.google.com/youtube/answer/15424877) |
| Content ID on Shorts | a Short over one minute with any active claim is blocked globally; Audio Library music is not claimed | same, [Audio Library](https://support.google.com/youtube/answer/3376882) |
| Partner Program | 1,000 subscribers plus 4,000 watch hours in 12 months or 10 million Shorts views in 90 days; expanded tier at 500 subscribers, 3 uploads in 90 days, and 3,000 hours or 3 million Shorts views | [YPP eligibility](https://support.google.com/youtube/answer/72851), [Expanded YPP](https://support.google.com/youtube/answer/13429240) |
| Inauthentic content | 15 July 2025 rename of "repetitious content"; content must be original and "not be mass-produced, generic, repetitive, or manipulative" | [Channel monetization policies](https://support.google.com/youtube/answer/1311392) |
| Self-harm policy | supportive, recovery-focused wording; resources in video and description; no methods; crisis resource panels may be added | [Suicide, self-harm policy](https://support.google.com/youtube/answer/2802245) |
| Analytics | `reports.query` with `views`, `likes`, `averageViewDuration`, `averageViewPercentage`, `subscribersGained`, filterable by `video`; scopes `yt-analytics.readonly` and `youtube.readonly` | [Reports: query](https://developers.google.com/youtube/analytics/reference/reports/query) |

## 11. Tools the owner asked about: HeyGen and ChatCut (checked 2026-10-08)

Joel sent `heygen.com` and `chatcut.io/claude`. Both were read on their own sites on 2026-10-08. Neither
changes the phase 1 plan; each has a place later, with conditions.

### HeyGen — AI presenter videos (a talking avatar reads the script)

**What it is.** A hosted service that renders a lip-synced AI presenter (stock "studio" avatars, a
"digital twin" of a real person, or an animated photo) speaking a script, plus video translation and a
"Video Agent" that builds a whole video from a prompt. It has a developer API.

| Fact | Value | Source |
|---|---|---|
| Web plans | Free: 3 videos a month, up to 1 minute, 1080p. Creator $29 a month ($24 annual): 600 credits, videos up to 30 minutes, watermark removal. Pro $49: 1,000 credits. Business $149 plus $20 a seat: 1,500 credits. | [Pricing](https://www.heygen.com/pricing), [FAQ](https://www.heygen.com/faq) |
| Credit burn on web plans | 20 credits a minute for Avatar IV, Avatar V and Video Agent; 3 a minute for Avatar III (so Creator's 600 credits are about 30 minutes of current-generation avatar video a month) | search summary of the pricing FAQ; not re-read directly, treat as approximate |
| API billing | separate pay-as-you-go wallet, starts at $5, no subscription, "price per 1 minute, charged by actual seconds generated"; **no free API credits since February 2026**; pay-as-you-go credits expire after 12 months; 10 concurrent videos | [API plans article, updated 2026-09-16](https://intercom.help/heygen/en/articles/10060327-new-heygen-api-plans), [FAQ](https://www.heygen.com/faq) |
| API per-minute rates | the official table did not render on the help article or the app page. One third-party page that could be read ([AdMake, published 2026-06-26, prices checked 2026-07-11](https://admakeai.com/blog/heygen-pricing-explained)) lists $1 a minute for a standard avatar video at 720p or 1080p, $4 a minute for Avatar IV at 1080p, and $2 a minute for Video Agent and for translation; search summaries of other pages put Avatar V at about $4 a minute. **Unverified on an official page; confirm in the wallet before budgeting.** | |
| API limits | 10 concurrent workflows on pay-as-you-go; `POST /v3/videos` 10 a second; avatar script up to 5,000 characters; 30 minutes a scene | [Usage limits](https://developers.heygen.com/docs/usage-limits) |
| API shape | `X-Api-Key` header; create, then poll `GET /v3/videos/{id}` until `completed`, or give a `callback_url`; result is a `video_url` MP4 | [Quick start](https://developers.heygen.com/docs/quick-start) |
| Ownership and commercial use | Creator, Pro and Business: "you own all rights in your User Input or User Output", commercial use allowed. **Free plan output is a revocable licence for personal, non-commercial and evaluation use and "may not be ... monetized, or used in connection with commercial activities."** The terms do not say which bucket pay-as-you-go API users fall in. | [Terms](https://heygen.com/terms) |
| AI disclosure | the terms require disclosing AI origin where the law requires and forbid presenting output as human-made; YouTube requires the synthetic-media flag for a realistic person saying things they did not say, which is exactly what an avatar is | [Terms](https://heygen.com/terms), `docs/DISTRIBUTION.md` |

**Fit for this channel.** HeyGen is a different format from the brief: the brief is faceless, and an
avatar is a face. It could still earn a place as an optional **rung F, "AI presenter"**, for one or two
series where a person speaking to camera beats b-roll (a "word for today" or an interview-prep tip).
What it costs at one video a day on the unverified third-party rates: about $1 a video on Avatar III
and $3 to $4 on Avatar IV or V, so roughly $30 to $120 a month, which is the whole phase-6 budget. The
Creator web plan at $29 covers about 30 one-minute videos a month, but the web app is not the API, and
driving the web app is not automation. Risks: the uncanny-valley problem is sharpest for prayer and
grief content; the synthetic-media flag is mandatory; and the free plan cannot be used for a public
channel at all. **Verdict: not before phase 6, and only after a side-by-side test on one series against
rung B.** Recorded as D-016.

### ChatCut — an AI video editor that an agent can drive, with a Claude Code plugin

**What it is.** A cloud video editor (web app, Windows and macOS desktop app) whose editing agent takes
plain-English instructions: import media, cut a timeline, transcribe and caption, add motion graphics,
generate video, voice-over, music and sound effects, and export. The page Joel linked,
[chatcut.io/claude](https://chatcut.io/claude), is the install guide for its **Claude Code plugin**,
which adds an MCP server (`plugin:chatcut:chatcut`, hosted at `api.chatcut.io`) and a skill. There is
also an older npm CLI, `@chatcut/skill`.

| Fact | Value | Source |
|---|---|---|
| Plugin hosts | Claude Code Desktop, the Codex desktop app, WorkBuddy; **not** claude.ai, ChatGPT's site, remote browser workspaces, or the ChatCut web editor. Sign-in is a browser authorisation flow. | [Agent plugin docs](https://chatcut.io/docs/agent-plugin), [install page](https://chatcut.io/claude) |
| CLI | `@chatcut/skill` 0.2.1 (2026-05-31, licence `UNLICENSED`): `chatcut submit --prompt ... --asset ...` normalises assets locally with FFmpeg, uploads, runs the editing agent, renders in the cloud and prints a signed download URL; first run opens a browser once, then "everything is silent"; `chatcut login` can rotate an API key; first run downloads about 450 MB of Chromium through `@remotion/renderer` | [npm registry record](https://registry.npmjs.org/@chatcut/skill) |
| Plans | Free: a one-time starting balance (the pricing page says 5 credits, the docs say 20) and a cumulative 60-minute cloud-export quota that never resets. Paid, billed annually: 200 credits a month for $42 (list $50), 400 for $49 (list $100), 800 for $98 (list $200); paid removes the export quota. Desktop local export does not count against the quota and offers 4K. | [Pricing](https://chatcut.io/pricing), [Free and Pro](https://chatcut.io/docs/free-and-pro), [Export limits](https://chatcut.io/docs/web-export-limits) |
| What credits buy | AI generation only; "manual editing, uploads, transcription, and export do not consume credits". Video per generated second: Seedance 2.0 0.28 (480p), 0.60 (720p), 1.32 (1080p); Seedance 2.5 about 0.40, 0.90, 2.22; Seedance 2.0 mini 0.056 (480p), 0.12 (720p); Kling 3.0 Standard 0.60 (720p), Pro 0.80 (1080p). Voice-over 0.28 to 0.80 credits per 1,000 characters. Music 0.18 a song. Sound effects 0.12 a second. | [Credits policy](https://chatcut.io/docs/credits-policy), [Generation credits](https://chatcut.io/docs/generation-credits) |
| Terms | the terms say you will not access the service "through automated or non-human means" and prohibit "any automated use of the system (scripts, data mining, robots)"; commercial use of your exports is allowed and referred to a usage policy that is not published at a findable URL; output ownership is not stated | [Terms](https://chatcut.io/terms) |

**What a 55-second short would cost in credits.** At the 400-credit plan ($49 a month, about $0.12 a
credit): voice-over for 900 characters is under 1 credit; a music bed 0.18; all scenes as AI video with
Kling 3.0 Pro at 1080p is 44 credits (about $5.40); the same with Seedance 2.0 mini at 720p is under 7
credits (about $0.80). So ChatCut is a reasonable **rung C or D provider**, priced in the same range as
calling the video models directly, with captions, music and editing thrown in.

**Fit for this channel.** Attractive as a one-stop rung-D renderer and as an interactive tool for
designing the brand look with Claude Code Desktop. Three things keep it out of the unattended pipeline for
now: the terms forbid automated use while the product ships a CLI and an agent plugin built for exactly
that (ask ChatCut in writing before relying on it); the plugin runs only in desktop agent hosts and the
CLI's licence is `UNLICENSED`; and the agent's edits are non-deterministic, which fights the
idempotent, downgradeable design in `docs/ARCHITECTURE.md`. **Verdict: use interactively in phase 1 to
prototype caption styles and brand cards, and reconsider as a rung-D provider in phase 6 once its
automation terms are confirmed.** Recorded as D-016.

## 3. Visuals: stock, AI images, AI video (checked 2026-10-08)

**Summary.** Three tiers with a clean cost gap between them. Stock clips are free and licence-clean
from two APIs. AI stills for a Ken Burns format cost a fraction of a cent to a few cents each, so eight
per video is roughly two to thirty cents. AI video costs about $0.04 to $0.10 per generated second on the
budget models and $0.40 on Google's top model with audio, so a 60-second short built from six to eight
clips is roughly $2.40 to $6.40 on budget models and over $25 on Veo 3.1 Standard. No video API has a
free tier. The market churns monthly (OpenAI's Sora 2 API was shut down on 2026-09-24; Google's original
Nano Banana image model was shut down on 2026-10-02), so every generator sits behind one adapter.

### Stock footage and images (rung B)

| Source | Price | Limits | Licence | Fit |
|---|---|---|---|---|
| [Pexels API](https://www.pexels.com/api/documentation/) | free | 200 requests an hour, 20,000 a month; "unlimited for free" on request; video search accepts `orientation=portrait` (confirmed on the docs page) | [free for commercial use, attribution not required, modification allowed](https://www.pexels.com/license/); the API terms ask for a visible Pexels link in the application, which a line in the README and daily summary satisfies | **primary** |
| [Pixabay API](https://pixabay.com/api/docs/) | free | 100 requests a minute; responses must be cached 24 hours; no hotlinking; "systematic mass downloads are not allowed" and the API is "intended for real human requests", so keep volume modest | [free, no attribution, modification allowed, no standalone resale](https://pixabay.com/service/license-summary/); covers its music too | **fallback** |
| [Coverr API](https://api.coverr.co/docs/start) | demo tier free at 50 requests an hour; production needs a paid plan | | [free for commercial use, no attribution](https://coverr.co/license) | spare |
| [Unsplash API](https://unsplash.com/documentation) | free | 50 an hour demo | photos only; its [guidelines](https://help.unsplash.com/en/articles/2511245-unsplash-api-guidelines) prohibit "automated uses" and require hotlinking | **excluded** |
| [Mixkit](https://mixkit.co/llm-info/) | free | no API | per-clip Free versus Restricted licence | excluded (cannot be automated safely) |
| [Storyblocks](https://www.storyblocks.com/business-solutions/api) | plans from $21 a month billed annually; API quote-only | | royalty-free with Content ID protection; one YouTube channel on individual plans | later, if stock repetition becomes the problem |

### AI images (rung C)

| Option | Price per image | Free | Notes |
|---|---|---|---|
| [FLUX.1 schnell on fal.ai](https://fal.ai/models/fal-ai/flux/schnell) | $0.003 per megapixel, rounded up, commercial rights included; [Replicate](https://replicate.com/pricing) $3 per 1,000 | none hosted; **$0 locally** (weights are [Apache-2.0](https://huggingface.co/black-forest-labs/FLUX.1-schnell)) in ComfyUI on a 4 GB NVIDIA card | **phase-6 default**; pick 704x1408 (0.99 MP) so fal bills one megapixel, not two |
| FLUX.1 dev and 1.1 pro ([fal](https://fal.ai/models/fal-ai/flux-pro/v1.1), Replicate, [Together](https://www.together.ai/pricing)) | $0.025 and $0.04 per megapixel or image | none; local dev weights are under a [non-commercial licence](https://huggingface.co/black-forest-labs/FLUX.1-dev) | better prompt adherence and text |
| [OpenAI GPT Image](https://developers.openai.com/api/docs/guides/image-generation) | `gpt-image-1-mini` low $0.006 for 1024x1536 portrait; `gpt-image-2` low $0.005 to high $0.165 | none | strongest text rendering; organisation verification may be required |
| [Google Nano Banana 2.1](https://ai.google.dev/gemini-api/docs/pricing) | $0.0336 at 1K; Pro $0.134 | **none**: the pricing page lists the free tier as "Not available" for every image model | SynthID watermark on every image |
| Ideogram 4.5 via [fal](https://fal.ai/models/fal-ai/ideogram/v3) or Pika's catalog | $0.027 low to $0.09 high (reseller prices; the official page is JavaScript-only) | weekly credits in the consumer app only | best typography for quote cards |
| [Recraft](https://www.recraft.ai/docs) | V4.1 Flash about $0.007 | none | vector output |
| Midjourney | subscription only, no API, automation prohibited per secondary sources | | **excluded** |
| Local UIs | [ComfyUI](https://github.com/comfyanonymous/ComfyUI) (GPL-3.0, weekly releases, Windows desktop app, JSON API for headless use) | $0 | [Fooocus](https://github.com/lllyasviel/Fooocus) (last commit 2025-09-02, SDXL only) and [Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge) (2025-06-26) are dormant |

### AI video (rung D)

Prices per generated second, 9:16 support as documented, and the cost of a 60-second short from 6 to 8
clips.

| Model | Route | Price | 9:16 | 60 s short | Notes |
|---|---|---|---|---|---|
| Veo 3.1 Lite | [Gemini API](https://ai.google.dev/gemini-api/docs/pricing) | $0.05 (720p) or $0.08 (1080p) with audio | yes | $3.20 | 4, 6 or 8 s clips; no free tier; SynthID; stored 2 days |
| Veo 3.1 Fast | Gemini API or [fal](https://fal.ai/models/fal-ai/veo3.1) | $0.10 (720p); fal $0.10 without audio | yes | $6.40 | |
| Veo 3.1 Standard | Gemini API | $0.40 with audio; fal $0.20 without | yes | $25.60 | the quality ceiling; turn audio off, the pipeline adds its own |
| Pika 2.5 | [Pika developer catalog](https://api.dev.pika.art/catalog/apis) | $0.04 (720p), $0.09 (1080p) | yes on [fal v2.2](https://fal.ai/models/fal-ai/pika/v2.2/text-to-video) | $2.40 | cheapest branded option; "no free generation tier" |
| Hailuo 02 and 2.3 | [fal](https://fal.ai/models/fal-ai/minimax/hailuo-2.3/standard/text-to-video) | $0.045 per s; 2.3 $0.28 per 6 s | undocumented in MiniMax's own API | $2.70 to $3.36 | official packages start at $1,000 a month, so use a reseller |
| Kling 2.5 Turbo Pro | [fal](https://fal.ai/models/fal-ai/kling-video/v2.5-turbo/pro/text-to-video) | $0.35 per 5 s plus $0.07 per extra second | yes (schema enum) | $4.20 | official packages from $700 with expiry |
| Runway Gen-4 Turbo | [Runway API](https://docs.dev.runwayml.com/guides/pricing/) | $0.05 | 720x1280 per help centre | $3.00 | Gen-4.5 $0.12 |
| Luma Ray 3.2 | [Luma API](https://lumalabs.ai/api/pricing) | 10 s at 720p $0.90; billed per 5 s block | yes | $5.40 | |
| Wan 2.2 (open weights) | [fal](https://fal.ai/models/fal-ai/wan/v2.2-a14b/text-to-video) or local | $0.04 (480p) to $0.08 (720p); local $0 | 704x1280 on the 5B model | $2.40 to $4.80 | [Apache-2.0](https://github.com/Wan-Video/Wan2.2); local needs a 24 GB card and about 9 minutes per 5 s clip |
| LTX-2 (open weights) | [fal](https://fal.ai/models/fal-ai/ltx-2/text-to-video) or local | $0.06 (1080p) | only 16:9 listed on fal | $3.60 | audio and video in one model; local set about 66 GiB |
| HunyuanVideo 1.5 | [fal](https://fal.ai/models/fal-ai/hunyuan-video-v1.5/text-to-video) | $0.075 (480p only) | yes | $4.50 | licence [excludes the EU, UK and South Korea](https://github.com/Tencent-Hunyuan/HunyuanVideo/blob/main/LICENSE.txt); local needs 45 to 60 GB |
| CogVideoX-2B | local | $0 | 720x480 landscape only | | the only video model for a small GPU, at low quality |
| Sora 2 | | **shut down 2026-09-24** per [OpenAI's docs](https://developers.openai.com/api/docs/guides/video-generation); resellers still list stale prices | | | excluded |

Hosting keys: [fal.ai](https://fal.ai/pricing) covers almost every model above behind one key;
[Replicate](https://replicate.com/pricing) is the backup. Neither advertises sign-up credit.

### What this section decides

- Rung B: Pexels primary, Pixabay fallback, clips downloaded and cached, a Pexels credit line in the
  README and the daily summary.
- Rung C: FLUX.1 schnell on fal (about $0.02 to $0.05 for eight images) or locally in ComfyUI; OpenAI's
  mini image model or Ideogram only for frames that need legible text.
- Rung D: one `VideoClipProvider` adapter with Veo 3.1 Lite, Pika 2.5 and Kling behind it; prefer
  image-to-video from the rung-C still so the Ken Burns fallback is identical; generate audio-off; pin
  clip lengths to each vendor's billing block (Veo 8 s, Kling and Luma 10 s).
- Record provider, model, licence tag, cost and watermark flag per clip in the job file so disclosure
  and licence questions can be answered later.

**Open questions.** Whether Together's free FLUX schnell endpoint still exists (announced as a 3-month
promotion in October 2024); Ideogram's and Kling's official prices (JavaScript-only pages); whether
Hailuo text-to-video can produce 9:16 at all; LTX-2's community licence terms.

### MiroFish — a swarm-simulation prediction engine (not a content tool)

**What it is.** [666ghj/MiroFish](https://github.com/666ghj/MiroFish) is an open-source "swarm
intelligence engine, predicting anything": you upload seed material (a news item, a draft, a story) and a
prediction question in plain language; it builds a knowledge graph, generates thousands of agent personas
with memory, runs them on two simulated social platforms, and returns a prediction report you can
interrogate by chatting with any simulated agent. Facts read on 2026-10-08:

| Fact | Value | Source |
|---|---|---|
| Licence and status | AGPL-3.0; Python; 77,118 stars and 11,788 forks; created 2025-11-26, last push 2026-10-01; 136 open issues; incubated by Shanda Group; simulation engine is CAMEL-AI's [OASIS](https://github.com/camel-ai/oasis) | GitHub repository record, [README](https://github.com/666ghj/MiroFish) |
| Running it | Node 18+, Python 3.11 or 3.12, `uv`; `npm run setup:all` then `npm run dev` (frontend on 3000, backend API on 5001), or `docker compose up -d`; Windows is not mentioned either way | README |
| What it needs | an OpenAI-compatible LLM key (the README recommends Alibaba's Qwen-plus and warns "High consumption, try simulations with fewer than 40 rounds first") and a [Zep Cloud](https://app.getzep.com/) key for agent memory ("free monthly quota is sufficient for simple usage") | README |
| Hosted version | [mirofish.ai](https://mirofish.ai/) shows only a title and, per search results, a waitlist for an online edition; no pricing | site fetch |
| Automation | a backend API exists for its own frontend; there is no documented headless or batch interface and the workflow is designed around interactive review | README, third-party guide |

**Fit for this channel.** MiroFish does not make videos, voices, captions or posts, so it has no place in
the production pipeline. The one honest use is as an **audience rehearsal**: feed it a candidate series
concept, a week of titles and hooks, or the wording of a sensitive mental-health script, and ask how a
simulated audience reacts before anything is published. Three reasons to treat that as a phase-4 or
later experiment rather than a stage: each run is thousands of LLM calls, so the cost of one rehearsal can
exceed a month of the rest of the $0 pipeline; the output is a prediction, while the analytics loop in
phase 4 measures the real audience for free; and it needs a cloud memory service and an interactive UI,
which fights the unattended design. The AGPL licence is fine for private use and only matters if it were
offered as a service. **Verdict: optional experiment after phase 4, capped by the budget guard, used to
rank series concepts, never to approve or reject a daily video.** Recorded as D-017.

## 4. Captions and music (checked 2026-10-08)

**Summary.** Because the voice is generated from a script the pipeline already has, the cheapest and most
accurate caption path is to take word timings from the voice engine and never transcribe. When an engine
gives no timings, force-align the known script rather than transcribing blind. Rendering is FFmpeg's
libass filter with a generated ASS file. For music, the only source YouTube itself promises is free of
Content ID claims is its own Audio Library, which says nothing about other platforms; a $10-a-month
safelisting subscription is the clean cross-platform answer once the channel is live on several
platforms; and among AI music generators only one has a documented API with commercial terms.

### Where word timings come from

| Voice engine | Timing output | Source |
|---|---|---|
| Kokoro via [Kokoro-FastAPI](https://github.com/remsky/Kokoro-FastAPI) | per-word timestamps from `/dev/captioned_speech` (the OpenAI-compatible route gives chunk-level only); the underlying [kokoro pipeline](https://github.com/hexgrad/kokoro) sets `start_ts`/`end_ts` on tokens for English only | README, `kokoro/pipeline.py` |
| [ElevenLabs](https://elevenlabs.io/docs/api-reference/text-to-speech/convert-with-timestamps) | character-level start and end times from the `with-timestamps` endpoint; group characters into words yourself | API reference |
| [Azure AI Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/how-to-speech-synthesis) | a `WordBoundary` event per word, punctuation and sentence with offset and duration; first-class .NET SDK | docs |
| [Google Cloud TTS](https://docs.cloud.google.com/text-to-speech/docs/reference/rest/v1beta1/text/synthesize) | timepoints only for SSML `<mark>` tags on the `v1beta1` endpoint; `v1` has none, and [Chirp 3 HD](https://docs.cloud.google.com/text-to-speech/docs/chirp3-hd) ignores `<mark>`, so word timing is effectively unavailable on Google's newest voices | reference pages |
| OpenAI TTS | no word timestamps (not confirmed on the reference page, which refused the fetch) | open question |

### Forced alignment and transcription fallbacks

| Tool | Licence | Status | Notes |
|---|---|---|---|
| [faster-whisper](https://github.com/SYSTRAN/faster-whisper) | MIT | 1.2.1 on 2025-10-31 | `word_timestamps=True`; CPU int8 is fine for 60-second clips; GPU on Windows needs cuBLAS and cuDNN from a third-party archive. **Local fallback.** |
| [WhisperX](https://github.com/m-bain/whisperX) | BSD-2 | 3.8.6 on 2026-05-25 | wav2vec2 forced alignment; heavier install; alignment models for en, fr, de, es, it |
| [stable-ts](https://github.com/jianfch/stable-ts) | MIT | **archived 2026-05-30** | `align()` of a known script plus ASS karaoke export; pin 2.19.1 if used |
| [whisper.cpp](https://github.com/ggml-org/whisper.cpp) | MIT | v1.9.5 (6 Oct) | word timestamps via `-ml 1` (experimental); what Remotion's installer wraps |
| [OpenAI `whisper-1`](https://developers.openai.com/api/docs/pricing) | | | $0.006 a minute with word granularity; the cheaper `gpt-4o-mini-transcribe` returns **no** word timestamps |
| [Deepgram](https://deepgram.com/pricing) | | | $200 free credit, then $0.0043 a minute; word start and end in the response; C# SDK |
| [AssemblyAI](https://www.assemblyai.com/pricing) | | | $50 free credit, then $0.15 an hour; word times in milliseconds |

### Rendering captions

- [FFmpeg `subtitles` filter](https://ffmpeg.org/ffmpeg-filters.html#subtitles-1) with libass: options `fontsdir`
  and `force_style`; one ASS `Dialogue` line per word, or `\k` karaoke tags, gives the word-highlight
  look with no Python dependency. **Windows:** escape the drive-letter colon
  (`subtitles='C\:/path/file.ass'`) or use a relative path; pass `fontsdir=`; libass has used DirectWrite
  without fontconfig since 0.13.0, and a "Fontconfig error" on fontconfig builds is worked around with the
  `FONTCONFIG_FILE` and `FONTCONFIG_PATH` variables.
- [Remotion captions](https://www.remotion.dev/docs/captions/create-tiktok-style-captions):
  `createTikTokStyleCaptions` pages word tokens; `@remotion/install-whisper-cpp` provides timings. Free
  under Remotion's licence for an individual; the upgrade for animated captions.
- MoviePy 2.x `TextClip` takes a font file path, no ImageMagick; slower than libass.
- No maintained open-source "Hormozi-style" captioner exists ([captacity](https://github.com/unconv/captacity)
  last released 2024-06; captify has three commits); [Submagic](https://www.submagic.co/pricing) is the
  hosted option from $12 a month annual. Generate the ASS file in-house.

### Music

| Source | Cost | Attribution | Content ID | Other platforms |
|---|---|---|---|---|
| [YouTube Audio Library](https://support.google.com/youtube/answer/3376882) | free | only on Creative Commons tracks (filter "Attribution not required") | "won't be claimed by a rights holder through the Content ID system" | the page is silent; no download API, so curate a folder by hand |
| [Pixabay Music](https://pixabay.com/service/faq/) | free | none | contributors may register tracks, so claims happen; keep the track URL and licence summary per video and dispute; a rejected second appeal can become a strike | yes under the Pixabay Content License |
| [Free Music Archive](https://freemusicarchive.org/License_Guide) | free | per track; only CC BY and CC0 are usable (BY-NC and BY-ND are not) | varies | yes for CC BY and CC0 |
| [Kevin MacLeod, Incompetech](https://incompetech.com/music/royalty-free/youtube-contentid.html) | free | exact CC BY 4.0 credit block in the description | the catalogue is deliberately in Content ID; every upload is claimed and released within 72 hours only if the credit is present | yes with credit |
| Uppbeat | free plan gives 3 downloads then 1 a month (search snippet; the site returned 429 to every fetch) | per-video credit | | cannot feed a daily channel |
| [Epidemic Sound](https://www.epidemicsound.com/pricing/), [Artlist](https://artlist.io/blog/artlist-personal-plan/) | about $10 a month on annual billing (third-party trackers and a 2021 Artlist post; the price cards are JavaScript-only) | none | channel safelisting; content published while subscribed stays cleared, content soundtracked after cancelling is exposed | one channel each on YouTube, Facebook, Instagram, TikTok |
| [ElevenLabs Music API](https://elevenlabs.io/docs/api-reference/music/compose) | 900 credits a minute of music (Starter $6 a month for downloads; Creator $22 for no attribution) | required on Free, none from Creator up | | the only generator with a documented API and [self-serve commercial terms](https://elevenlabs.io/eleven-music-model-specific-terms); `force_instrumental` for beds |
| [Suno](https://suno.com/pricing) | Pro $8 a month for commercial rights | | | **no public API**, and the [terms](https://suno.com/terms) ban scraping; unofficial wrappers are a violation |
| Udio | | | | **excluded**: downloads disabled after the UMG settlement per late-2025 reports |
| [MusicGen](https://github.com/facebookresearch/audiocraft) | free | | | **excluded**: weights are CC-BY-NC |
| [Stable Audio Open 1.0](https://huggingface.co/stabilityai/stable-audio-open-1.0) | free under the community licence below $1M revenue | | | 47-second clips, better at sound effects than music; loopable for ambient beds |

Two platform notes from search snippets, to verify before phase 5: Meta's Sound Collection is licensed
only inside Facebook and Instagram, and TikTok's own library caps music at 60 seconds, so the music is
always baked into the MP4 before upload.

### What this section decides

- Captions come from Kokoro-FastAPI's captioned endpoint at the $0 tier, from the paid engine's own
  timings later, and from `faster-whisper` forced alignment only as a fallback. The script is ground
  truth; the pipeline never transcribes blind.
- Render with FFmpeg and a generated ASS file; Remotion only for animated page-style captions.
- Music: a curated local library of 20 to 30 tracks from the YouTube Audio Library (attribution-free
  filter) and Pixabay, with licence URL and attribution stored per track and written into the job file;
  Kevin MacLeod only with the credit block templated into the description. Move to a $10-a-month
  safelisting subscription in phase 5 when three platforms are live. For a generated bed per video, ACE-Step
  1.5 (MIT, local, under 4 GB of VRAM; section 12) is the $0 option and ElevenLabs Music the hosted one.
- Add a post-publish claim check to the track stage: poll each new YouTube video for Content ID claims
  and either replace the track or dispute with the stored licence text, and never let claims accumulate
  silently.

**Open questions.** Whether the Audio Library's standard licence allows the same tracks on TikTok and
Instagram (read the in-Studio licence text); current Epidemic Sound, Artlist and Uppbeat prices (pages
are JavaScript-only or rate-limited); whether whisper.cpp ships Windows binaries.

## 12. Hugging Face: models, Spaces, datasets and the free GPU minutes (checked 2026-10-08)

Joel asked for a look at Hugging Face. The Hub was searched through its own API on 2026-10-08 for every
pipeline stage; licences, dates and download counts below are from the model and dataset records, and the
platform rules are from Hugging Face's documentation. Four things change the plan: a licence-clean local
music generator exists, a newer Apache-licensed voice with emotion control exists, an Apache-licensed image
model with good text rendering exists, and a free account gets five GPU minutes a day on shared Spaces,
which is enough to render a short's images or one clip without owning a GPU.

### The platform

| Fact | Value | Source |
|---|---|---|
| Inference Providers | one Hugging Face token reaches fal, Replicate, DeepInfra, WaveSpeed and others at pass-through prices with "no markup"; **free accounts get no monthly credits** and must buy them; PRO accounts get $2 a month of compute credits | [Pricing and billing](https://huggingface.co/docs/inference-providers/pricing) |
| ZeroGPU Spaces | any public Gradio Space on shared NVIDIA RTX Pro 6000 Blackwell GPUs (48 GB) can be called as an API; included daily GPU quota: 2 minutes unauthenticated, **5 minutes for a free account**, 40 minutes for PRO (extensible at $1 per 10 minutes); the quota resets 24 hours after first use; a free account in good standing (verified email, older than 30 days) may **host two ZeroGPU Spaces of its own** | [Spaces ZeroGPU](https://huggingface.co/docs/hub/spaces-zerogpu) |
| Calling a Space | every Gradio Space is an API: `gradio_client` in Python, `@gradio/client` in JavaScript, or two curl calls against `/gradio_api/call/<endpoint>`; an OpenAPI spec is served at `<space>.hf.space/gradio_api/openapi.json`; authenticating with a token spends your own quota and gets better rate limits; many Spaces also expose an MCP server (`mcp-server` tag) | [Spaces as API endpoints](https://huggingface.co/docs/hub/spaces-api-endpoints) |
| PRO | $9 a month: 8x ZeroGPU quota and highest queue priority, $2 of inference credits, up to 10 hosted ZeroGPU Spaces | [Pricing](https://huggingface.co/pricing) |

What this buys the project: a **GPU-free host** (the owner's PC without a graphics card, or GitHub Actions
in Concept 2) can still run rung C and rung D by calling a ZeroGPU Space, within five free minutes a day,
or by routing to fal or Replicate through Inference Providers at the same prices as section 3.

### Voice

| Model | Licence | Activity | Notes |
|---|---|---|---|
| [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) | Apache-2.0 | 129M downloads; live on fal and DeepInfra through Inference Providers | the phase-1 primary (D-007); demo at [hexgrad/Kokoro-TTS](https://huggingface.co/spaces/hexgrad/Kokoro-TTS) |
| [Qwen3-TTS 1.7B CustomVoice](https://huggingface.co/Qwen/Qwen3-TTS-12Hz-1.7B-CustomVoice) and the 0.6B variants | Apache-2.0 | released 2026-01-21, 16.5M downloads | nine preset voices with a natural-language `instruct` for tone and emotion ("warm, unhurried"), a VoiceDesign model that builds a voice from a description, and a Base model for 3-second cloning; ten languages; `pip install qwen-tts`; the README's examples assume a CUDA GPU in bfloat16, and GGUF conversions exist for CPU. **Added to the D-007 listening test** as the Apache option with emotion control that Kokoro lacks. |
| [Chatterbox](https://huggingface.co/ResembleAI/chatterbox) | MIT | 23M downloads; fal, Replicate, DeepInfra | the expressive option from section 2; demos at [chatterbox-turbo-demo](https://huggingface.co/spaces/ResembleAI/chatterbox-turbo-demo) |
| [VibeVoice-1.5B](https://huggingface.co/microsoft/VibeVoice-1.5B) (Microsoft) | MIT | 3.7M downloads | long-form, podcast-style multi-speaker English and Chinese; more than this channel needs |
| [Fun-CosyVoice3-0.5B](https://huggingface.co/FunAudioLLM/Fun-CosyVoice3-0.5B-2512) | Apache-2.0 | 2025-12 | nine languages, ONNX available |
| [OmniVoice](https://huggingface.co/k2-fsa/OmniVoice), [VoxCPM2](https://huggingface.co/openbmb/VoxCPM2), [XTTS-v2](https://huggingface.co/coqui/XTTS-v2) | no licence tag, no licence tag, non-commercial | | not used until a licence is stated |
| Leaderboards | | | [open_tts_leaderboard](https://huggingface.co/spaces/hf-audio/open_tts_leaderboard) and [TTS-Spaces-Arena](https://huggingface.co/spaces/Pendrokar/TTS-Spaces-Arena) for the bake-off |

### Images (rung C)

| Model | Licence | Activity | Notes |
|---|---|---|---|
| [FLUX.1-schnell](https://huggingface.co/black-forest-labs/FLUX.1-schnell) | Apache-2.0 (gated: accept the terms once) | 23.3M downloads; live on nscale, fal, WaveSpeed | section 3's default |
| [Z-Image-Turbo](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo) (Alibaba Tongyi) | Apache-2.0 | released 2025-11-25, 9.3M downloads; fal, Replicate, WaveSpeed | 6B parameters, 8 steps, "fits comfortably within 16G VRAM consumer devices", photorealism and English and Chinese text rendering. **The second rung-C model**, and the one to try first for quote cards because of the text rendering. Demo at [Tongyi-MAI/Z-Image-Turbo](https://huggingface.co/spaces/Tongyi-MAI/Z-Image-Turbo). |
| [Ming-Image-0.1-Design](https://huggingface.co/inclusionAI/Ming-Image-0.1-Design) | MIT | 2026-09-17 | 6B model for posters, infographics and other text-rich design with transparent-background output; validated on an 80 GB GPU, so only through a hosted Space ([demo](https://huggingface.co/spaces/hugging-apps/ming-image-0-1-design-demo)); interesting for scripture cards later |
| [Qwen-Image-2.1](https://huggingface.co/Qwen/Qwen-Image-2.1), [Krea-2-Turbo](https://huggingface.co/krea/Krea-2-Turbo), [FLUX.1-dev](https://huggingface.co/black-forest-labs/FLUX.1-dev) | "other" or non-commercial | the trending models of the moment | read the licence before any commercial use; not adopted |

### Video (rung D)

| Model | Licence | Hardware | Notes |
|---|---|---|---|
| [Wan2.1-T2V-1.3B](https://huggingface.co/Wan-AI/Wan2.1-T2V-1.3B) | Apache-2.0 | "requires only 8.19 GB VRAM", a 5-second 480p clip in about 4 minutes on an RTX 4090 | **the consumer-GPU local option** section 3 was missing; 480p output needs upscaling for 1080x1920 |
| [Wan2.2-TI2V-5B](https://huggingface.co/Wan-AI/Wan2.2-TI2V-5B) | Apache-2.0 | 24 GB | from section 3; live on fal, Replicate, WaveSpeed |
| [LTX-2](https://huggingface.co/Lightricks/LTX-2), [LTX-2.3](https://huggingface.co/Lightricks/LTX-2.3), [LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | "other" (community licence) | large; GGUF quantisations and ComfyUI workflows exist | the most active open video family on the Hub (LTX-2.5: 3.4M downloads since July 2026, gated); audio and video together; licence terms must be read |
| [MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3) | "other" | 33B parameters | the trending model of the week with synchronised audio; hosted only (fal, WaveSpeed) |
| [SANA-Video 2.0 5B 720p](https://huggingface.co/Efficient-Large-Model/SANA-Video_2.0_5B_720p) | Apache-2.0 | | 2026-08; worth a test |
| [LongCat-Video](https://huggingface.co/meituan-longcat/LongCat-Video) | MIT | | live on fal |
| Free Spaces with an API | | within the ZeroGPU quota | [Wan2.2 14B Fast](https://huggingface.co/spaces/zerogpu-aoti/wan2-2-fp8da-aoti-faster) (image-to-video, 3,710 likes, MCP server), [LTX Video Fast](https://huggingface.co/spaces/Lightricks/ltx-video-distilled) (MCP server). One hook clip a day fits in five free minutes. |

### Music

| Model | Licence | Hardware | Notes |
|---|---|---|---|
| [ACE-Step 1.5](https://huggingface.co/ACE-Step/Ace-Step1.5) | **MIT**, with the model card stating it is "designed for creators" and that generated music may be used "strictly ... for commercial purposes", trained on licensed, royalty-free and synthetic data | "runs locally with less than 4GB of VRAM"; a full song in under 10 seconds on an RTX 3090; the GitHub README lists a **Windows portable package**, a REST API server (`uv run acestep-api`) and CPU support | released 2026-01-23, 453K downloads. **This changes section 4's conclusion:** an instrumental bed can be generated locally for $0 under a permissive licence, which no other generator offered. Demo at [Ace-Step-v1.5](https://huggingface.co/spaces/ACE-Step/Ace-Step-v1.5); the earlier [ACE-Step v1 3.5B](https://huggingface.co/ACE-Step/ACE-Step-v1-3.5B) is Apache-2.0. |
| [Stable Audio Open 1.0](https://huggingface.co/stabilityai/stable-audio-open-1.0) | community licence (gated) | | 47-second clips, from section 4 |
| [MiniMax-Music3](https://huggingface.co/MiniMaxAI/MiniMax-Music3), [YuE2-3B](https://huggingface.co/m-a-p/YuE2-3B), [MusicGen](https://huggingface.co/facebook/musicgen-large) | no licence tag, CC-BY-NC, CC-BY-NC | | not usable |

A generated bed still needs the same care as a downloaded one: keep the prompt, seed and model version
with the job so a Content ID dispute has evidence, and run the post-publish claim check.

### Speech recognition for alignment

[nvidia/parakeet-tdt-0.6b-v3](https://huggingface.co/nvidia/parakeet-tdt-0.6b-v3) (CC-BY-4.0, 25
European languages, word timestamps through NeMo, 3.2M downloads, live on Together) is a fast alternative
to Whisper for the forced-alignment fallback in section 4.

### Datasets

| Dataset | What it is | Notes |
|---|---|---|
| [k-mktr/world_english_bible_en](https://huggingface.co/datasets/k-mktr/world_english_bible_en) | the World English Bible, 29.7K verse rows, 1.8 MB parquet, columns for book, chapter, verse number, verse id and text | public-domain translation; **the scripture lookup file for phase 3** |
| [k-mktr/berean_standard_bible_en](https://huggingface.co/datasets/k-mktr/berean_standard_bible_en) | the Berean Standard Bible, 31.1K rows, 1.3 MB, same schema | public domain (CC0) |
| [JDRJ/kjv-bible](https://huggingface.co/datasets/JDRJ/kjv-bible) | the King James Version, 31.1K rows, 2.3 MB, book, chapter, verse, text | public domain outside the UK |
| [geosfero/positivequotation-public-domain-quotes](https://huggingface.co/datasets/geosfero/positivequotation-public-domain-quotes) | 30 proverbs, each matched to a numbered entry in a public-domain source, with source title, compiler and URL | tiny, but the **model for the verified quote file**: every row carries its provenance |
| [Abirate/english_quotes](https://huggingface.co/datasets/Abirate/english_quotes), [asuender/motivational-quotes](https://huggingface.co/datasets/asuender/motivational-quotes), [jstet/quotes-500k](https://huggingface.co/datasets/jstet/quotes-500k), [c2p-cmd/Good-Quotes-Authors](https://huggingface.co/datasets/c2p-cmd/Good-Quotes-Authors) | scraped quote collections (Goodreads and similar) with author labels | **candidates only, never sources**: user-submitted attributions are exactly the misattribution problem in `docs/CONTENT_STRATEGY.md`; a quote from these enters the verified file only after a human finds the primary source |

### What this section decides

- Music: add ACE-Step 1.5 as the generated-bed option at $0, local, licence-clean (D-018). The curated
  library from section 4 remains the launch default because it needs no GPU.
- Voice: Qwen3-TTS joins Kokoro, Chirp 3 HD, `gpt-4o-mini-tts` and ElevenLabs in the D-007 listening test.
- Images: Z-Image-Turbo is the second rung-C model and the first to try for text-bearing cards.
- Video: Wan2.1-T2V-1.3B is the local option for an 8 GB card; the ZeroGPU Spaces are the $0 hosted
  option for one clip a day.
- Hosting: a GPU-free host can call a ZeroGPU Space (5 free minutes a day, 40 on PRO at $9 a month) or
  route to fal and Replicate through Inference Providers with one token; the owner may also host two
  ZeroGPU Spaces of his own for free, which is a way to run Kokoro, Z-Image-Turbo or ACE-Step as a private
  API without a local GPU.
- Scripture: the three public-domain translations are downloaded once as parquet files into `assets/`.

**Open questions.** Qwen3-TTS speed on CPU and whether it returns timestamps; whether the LTX community
licence permits this use; ACE-Step 1.5 quality for calm instrumental beds (its benchmark claims are
song-oriented); which Spaces stay up, since a Space is someone's hobby unless it belongs to the model's
authors.
