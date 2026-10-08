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
| API per-minute rates | the official table did not render on the help article or the app page. Third-party pages agree on roughly $1 a minute for Avatar III, $3 to $4 for Avatar IV (photo avatar cheaper than studio or digital twin), about $4 for Avatar V, $2 for Video Agent ([G2](https://www.g2.com/articles/heygen-api-pricing), [AdMake](https://admakeai.com/blog/heygen-pricing-explained)). **Unverified on an official page; confirm in the wallet before budgeting.** | |
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
