# Research — tools, prices, APIs and policies, with links and dates

Every price, quota and licence term below was read on the linked page on the date in the section
heading unless a line says otherwise. Prices change; the roadmap's phase 7 re-checks this file
quarterly. "Impression" marks a quality judgement that was not measured. Where two sources disagreed,
both are shown.

How this was gathered: nine research passes, one for each of sections 1 to 9, each followed by a second,
adversarial pass that re-fetched the numeric and policy claims and tried to refute them, and a final pass
that looked for gaps. Sections 10 to 12 were compiled directly: the cost sheets from the other sections'
prices, and the HeyGen, ChatCut, MiroFish and Hugging Face sections from the owner's follow-up questions.
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
11. Tools the owner asked about: HeyGen, ChatCut and MiroFish
12. Hugging Face: models, Spaces, datasets and the free GPU minutes

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

**Summary.** At 130 to 160 words a video (about 700 to 900 characters) and one to three videos a day,
roughly 21,000 to 81,000 characters a month, voice is effectively free at every tier. What differs is licence
safety, naturalness, emotion control, and whether it runs on a Windows CPU.

| Option | Type | Price | Free allowance | Commercial use | Windows CPU | Notes |
|---|---|---|---|---|---|---|
| [Kokoro-82M](https://huggingface.co/hexgrad/Kokoro-82M) via [Kokoro-FastAPI](https://github.com/remsky/Kokoro-FastAPI) or [kokoro-onnx](https://github.com/thewh1teagle/kokoro-onnx) | open weights, local | $0 | unlimited | yes, Apache-2.0 weights; the author notes it "has been deployed in commercial APIs" | yes: `pip install kokoro` plus the espeak-ng MSI; FastAPI ships `start-cpu.ps1` and a CPU Docker image; about 3.5 s first-token on an older i7 (batch is fine) | 54 voices, 8 languages (v1.0, 2025-01-27); FastAPI exposes an OpenAI-compatible endpoint with caption timestamps; no emotion control; `kokoro` pins Python below 3.13. **Phase 1 primary (D-007).** |
| [Google Cloud Text-to-Speech](https://cloud.google.com/text-to-speech/pricing) | cloud | Chirp 3 HD $30 per 1M chars after the free 1M a month; Neural2 $16; Standard and WaveNet $4 after 4M free | 1M chars a month on current voices, 4M on Standard; a billing account must exist and overage is charged | yes under standard cloud terms (service-specific terms not fetched) | yes, official SDKs | about 1,000 scripts a month inside the free allowance. **Phase 1 fallback (D-007).** |
| [Gemini API TTS](https://ai.google.dev/gemini-api/docs/speech-generation) (`gemini-3.8-flash-tts`, `gemini-3.8-flash-lite-tts`) | cloud; a different product from Google Cloud TTS, used with an AI Studio key and no Cloud billing | $0.50 per 1M text tokens in and $9 (Flash) or $6 (Flash-Lite) per 1M audio tokens out, both doubling from 2027-01-01 | **"Free of charge"** on the Flash and Flash-Lite TTS models per the [pricing page](https://ai.google.dev/gemini-api/docs/pricing); the Pro preview has none; free-tier limits are shown only inside AI Studio, and free-tier prompts may be used to improve Google's products | yes under the API terms; the page states no disclosure rule of its own (YouTube's synthetic-media flag applies regardless) | API | 30 prebuilt voices, a turn-level `style` prompt, inline tags such as `<short pause>`, 24 kHz WAV out; **no word timestamps**, so captions need forced alignment (section 4). The cheapest expressive hosted voice; added after the verification pass flagged that only the Cloud product had been checked. |
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
fallback, Gemini Flash TTS's free tier as the $0 expressive option to test, `edge-tts` for prototyping
only, OpenAI or ElevenLabs Starter as the phase-6 upgrade for emotion. Before committing to a voice, run
a ten-script listening test across Kokoro (`af_heart`, `am_michael`), Chirp 3 HD, Gemini 3.8 Flash TTS,
`gpt-4o-mini-tts` and ElevenLabs; arena rankings could not be fetched (tables render
client-side), so quality claims above are vendor claims or impressions.

**Open questions.** Audio tokens per minute for `gpt-4o-mini-tts` (not published); whether ElevenLabs'
with-timestamps endpoint is on Starter; Kokoro's real-time factor on a typical desktop CPU (measure in the
bake-off); whether Polly's 12-month allowances apply to new accounts.

**Still unmeasured (raised by the verification pass, not re-read).** Windows 11's own natural voices
through `Windows.Media.SpeechSynthesis`, the one Microsoft voice path with no terms question, were not
evaluated; which voices that API exposes depends on the build. Per-request input caps reported by the
verifier (Google Cloud 5,000 bytes, OpenAI 4,096 characters, Polly 3,000 billed characters) are all above
a 700-character script, so they do not bind. Kokoro and Piper take plain text, so pauses come from
punctuation and the bake-off must check how each voice reads a scripture reference such as "Psalm 23:4".
Other permissively licensed open models were not surveyed: Orpheus-TTS, Dia, CosyVoice, VibeVoice, Zonos
and Sesame CSM-1B (licences unverified; Qwen3-TTS is in section 12). The owner's GPU, if any, is not
known, which is what decides whether the GPU-only local voices are even candidates.

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
| Analytics | `reports.query` with `views`, `engagedViews`, `likes`, `averageViewDuration`, `averageViewPercentage`, `subscribersGained` (among others), filterable by the `video` dimension; retention via `elapsedVideoTimeRatio` and `audienceWatchRatio`; day reports omit the most recent days; scopes `yt-analytics.readonly` and `youtube.readonly` | [Reports: query](https://developers.google.com/youtube/analytics/reference/reports/query), [Metrics](https://developers.google.com/youtube/analytics/metrics) |
| Spam policy | "automated or synthetic mass-production" is prohibited; the example is channels with "the exact same background music and repetitive AI generated imagery across many videos" and an AI-written narration; strikes apply, three in 90 days can terminate | [Spam policies](https://support.google.com/youtube/answer/2801973) |
| Quota history | upload cost changed from about 1,600 units to about 100 on 2025-12-04 and to its own bucket on 2026-06-01; `videos.update` and `thumbnails.set` cost 50 | [Revision history](https://developers.google.com/youtube/v3/revision_history), [Quota costs](https://developers.google.com/youtube/v3/determine_quota_cost) |
| February 2027 | new applicants: 8,000 qualified watch hours or 20 million qualified Shorts views plus 1,000 subscribers; Shorts pool earnings need 10 million qualified Shorts views per 90 days; active-channel rule of five Shorts per 90 days; creators keep 45 percent of allocated Shorts revenue | [Updates to YPP](https://support.google.com/youtube/answer/12843009), [Google blog 2026-08-11](https://blog.google/intl/en-mena/product-updates/connect-communicate/new-opportunities-to-earn-and-changes-to-the-youtube-partner-program/) |
| Unverified app | 100 users over the project lifetime on sensitive scopes; sensitive-scope verification needs a homepage, a privacy policy on an owned domain and a demo video, about 10 business days | [Publishing status](https://support.google.com/cloud/answer/15549945), [Verification FAQ](https://support.google.com/cloud/answer/13463817), [Sensitive scope verification](https://support.google.com/cloud/answer/13464321) |
| Shorts thumbnails | custom Shorts thumbnails opened to Partner Program channels first on 2026-07-24; custom thumbnails need the phone-verified intermediate feature level | [YouTube blog](https://blog.youtube/news-and-events/youtube-studio-custom-thumbnail-updates/), [Channel features](https://support.google.com/youtube/answer/9890437) |
| Encoding | MP4, `moov` atom first, H.264 High, closed GOP, 8 Mbps at 1080p 30 fps, AAC-LC 48 kHz 384 kbps | [Upload encoding settings](https://support.google.com/youtube/answer/1722171) |
| Browser automation | YouTube's terms forbid automated access without permission; uploader bots exist and break on login challenges; never on the main channel | see `docs/DISTRIBUTION.md` |

## 6. Other platforms and schedulers (checked 2026-10-08)

**Summary.** Every platform in scope has a free HTTPS publishing API that works from Windows; what decides
automation is each platform's access rules, not code. Meta's three surfaces are the friendliest for a
single owner: Instagram Reels through the Instagram-Login flavour of the API needs no Facebook Page and
Standard Access is enough for your own professional account; Facebook Reels post to a Page; Threads
publishes with no App Review once the owner is added as a tester. All three fetch the MP4 from a public
URL, so the pipeline needs public object storage. Bluesky needs no registration at all. Pinterest and
LinkedIn are automatable but gated by paperwork. TikTok is the real blocker: unaudited apps post
privately only, and TikTok's own guidelines list "a utility tool to help upload contents to the
account(s) you or your team manages" as unacceptable for the audit, so a personal headless tool should
not expect to pass. The cheapest way to make TikTok hands-off is an aggregator with pre-approved apps.

### Direct platform APIs

| Platform | Access for one owner | Daily ceiling | Video rules | Catch | Source |
|---|---|---|---|---|---|
| **Instagram Reels** | Instagram API with Instagram Login: professional account, **no Facebook Page needed**, Standard Access suffices "if the app only serves your Instagram professional account"; scopes `instagram_business_basic`, `instagram_business_content_publish` | 100 API posts per 24 h (the carousel section says 50) | MP4 or MOV, `moov` first, H.264 or HEVC, AAC 48 kHz, 23 to 60 fps, max 1920 wide, 9:16 recommended, 3 s to 15 min, 300 MB | the video is fetched from a public URL; long-lived tokens last 60 days, so a refresh job is needed | [Overview](https://developers.facebook.com/docs/instagram-platform/overview), [Instagram Login](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login), [Content publishing](https://developers.facebook.com/docs/instagram-platform/content-publishing) |
| **Facebook Reels** | Page access token with `pages_show_list`, `pages_read_engagement`, `pages_manage_posts`; Pages only | 30 API-published Reels per 24 h | 9:16, 1080x1920, 24 to 60 fps, **3 to 90 s**, H.264 or HEVC, closed GOP 2 to 5 s, AAC 48 kHz | a hosted `file_url` must allow the `facebookexternalhit/1.1` user agent; scheduling 10 minutes to 29 days ahead | [Reels publishing](https://developers.facebook.com/docs/video-api/guides/reels-publishing) |
| **Threads** | Meta app with the Threads use case; add yourself as a **Threads Tester** and publish without App Review; `threads_basic`, `threads_content_publish` | 250 posts per 24 h | MP4 or MOV, max 1920 wide, 9:16 recommended, up to 300 s, 1 GB | fetched from a public URL; wait about 30 s after creating the container; tokens 60 days | [Posts](https://developers.facebook.com/docs/threads/posts), [Get started](https://developers.facebook.com/docs/threads/get-started) |
| **Bluesky** | open protocol, no app registration or review; email-verified account | 25 videos and 10 GB a day at launch (September 2024), "we may tweak this limit"; check `getUploadLimits` | `video/mp4` up to 300 MB, aspect ratio required, up to 20 VTT caption files | upload goes to `video.bsky.app` with a service-auth token | [Video tutorial](https://raw.githubusercontent.com/bluesky-social/bsky-docs/main/docs/tutorials/video.mdx), [embed.video lexicon](https://raw.githubusercontent.com/bluesky-social/atproto/main/lexicons/app/bsky/embed/video.json), [launch post](https://bsky.social/about/blog/09-11-2024-video) |
| **TikTok** | Direct Post needs an audited app; unaudited apps post `SELF_ONLY` with at most 5 posting users per 24 h; the Upload (inbox) route needs no audit but a human must open the inbox notification and finish the post | about 15 posts a day per creator across all clients; 6 requests a minute per token | MP4 preferred, 23 to 60 fps, 360 to 4096 px, 4 GB | the [content sharing guidelines](https://developers.tiktok.com/doc/content-sharing-guidelines) list "a utility tool to help upload contents to the account(s) you or your team manages" as unacceptable; app review needs a demo video, a public website with privacy and terms pages, and "must not be for private or personal use"; `PULL_FROM_URL` needs a verified domain with no redirects | [Get started](https://developers.tiktok.com/doc/content-posting-api-get-started), [Upload](https://developers.tiktok.com/doc/content-posting-api-get-started-upload-content), [App review](https://developers.tiktok.com/doc/app-review-guidelines), [Direct Post](https://developers.tiktok.com/doc/content-posting-api-reference-direct-post) |
| **Pinterest** | Trial access by application; **Trial Pins are sandbox entities visible only to their creator**; Standard access needs a demo video of the OAuth flow "even if you are the only intended user" and an app tied to a Business account | per-day app limit on Trial, unspecified | MP4, MOV or M4V, 4 s to 15 min, 2 GB, 9:16 recommended | apps registered on or after 2026-09-14 may be refused for non-business users | [Access tiers](https://developers.pinterest.com/docs/key-concepts/access-tiers/), [FAQ](https://community.pinterest.biz/t/frequently-asked-questions-pinterest-api/2083), [Product specs](https://help.pinterest.com/en/business/article/pinterest-product-specs) |
| **X** | pay-per-usage credits only: $0.015 per post create ($0.20 if the post contains a URL), $20 of starter credit with a saved card; no free tier for new developers | 3 million post reads a cycle before Enterprise | | needs a card; media upload cost not stated on the pricing page; even aggregators now require your own X app | [Pricing](https://docs.x.com/x-api/getting-started/pricing), [About](https://docs.x.com/x-api/getting-started/about-x-api) |
| **LinkedIn** | "Share on LinkedIn" (`w_member_social`, legacy `ugcPosts`, doc last updated 2023-12-14) for a personal profile, or the Community Management API Development tier (500 calls an app a day, 100 a member, 12 months to finish integration) | 150 requests a member a day on the legacy path | 3 s to 30 min, up to 500 MB on the spec page | versioned headers required; the 202510 API version sunsets 2026-10-15 | [Share on LinkedIn](https://learn.microsoft.com/en-us/linkedin/consumer/integrations/self-serve/share-on-linkedin), [Access tiers](https://learn.microsoft.com/en-us/linkedin/marketing/increasing-access), [Videos API](https://learn.microsoft.com/en-us/linkedin/marketing/community-management/shares/videos-api) |

### Schedulers and aggregators

| Service | Price | TikTok without your own audited app | API | Notes | Source |
|---|---|---|---|---|---|
| [Postiz](https://github.com/gitroomhq/postiz-app) self-hosted | free, AGPL-3.0; Postgres, Redis and storage on your box | **no**: its TikTok provider forces `SELF_ONLY` until TikTok audits *your* app | yes, 90 requests an hour by default | 36.9k stars, v2.25.0 on 2 Oct; "you create your own developer apps on each platform and go through their approval (Meta, YouTube, TikTok can take weeks)"; Cloud from $29 a month uses pre-approved apps | [README](https://github.com/gitroomhq/postiz-app), [TikTok provider](https://docs.postiz.com/providers/tiktok), [Public API](https://docs.postiz.com/public-api), [Cloud pricing](https://postiz.com/pricing) |
| [Mixpost](https://mixpost.app/pricing) | Lite free (MIT) but only Facebook Pages, X and Mastodon; Pro $299 one-time for Instagram, YouTube, TikTok, Pinterest, Threads, Bluesky, LinkedIn and an API | no, same constraint | Pro and up | Laravel app; TikTok direct post "may require an additional audit" | [Pricing](https://mixpost.app/pricing), [TikTok guide](https://docs.mixpost.app/services/social/tik-tok/), [API](https://docs.mixpost.app/api/) |
| [Buffer](https://buffer.com/pricing) | Free: 3 channels, 10 queued posts a channel, API key with 3,000 requests a month; Essentials $5 a channel a month | **yes**, Buffer's own approved apps | yes | channels include TikTok, Instagram, Facebook, YouTube Shorts, Threads, Pinterest, Bluesky, LinkedIn, X; one post a day through the API keeps the queue under ten. **The $0 TikTok bridge.** | [Pricing](https://buffer.com/pricing) |
| [upload-post](https://www.upload-post.com/llms-full.txt) | Free 10 uploads a month without TikTok; Basic $24 a month ($16 annual) unlimited uploads, 5 profiles, 22 platforms | **yes**: "no TikTok developer app or audited-client review needed" and no Meta app review either | yes, one REST call with the file or a URL | the service MoneyPrinterTurbo uses; Make users report occasional unknown final status on heavy files | same |
| [Blotato](https://www.blotato.com/pricing) | Starter $29 a month: 20 accounts, up to 900 TikTok posts a month, API, n8n and Make nodes, hosted MCP | yes | yes | markets itself as avoiding "OAuth apps to get approved" | same |
| [Ayrshare](https://www.ayrshare.com/pricing/) | Premium $149 a month (1 profile, 14 networks, unlimited posts) | yes, except X now needs your own app | yes | TikTok caps apply (6 a minute, 15 a day) | [Pricing](https://www.ayrshare.com/pricing/), [TikTok notes](https://www.ayrshare.com/docs/apis/post/social-networks/tiktok) |
| [Publer](https://publer.com/help/en/article/what-are-publers-plans-and-pricing-15h4yqh/) | Free: 3 accounts, 10 pending posts an account; Professional from $5 an account | yes | only for eligible Business customers | no API on the free plan | same |
| [Metricool](https://metricool.com/pricing/) | Free: 1 brand, 20 posts a month, no LinkedIn or X; Starter from $20 | yes | only on Advanced and above | the free plan is ten posts short of daily | same |
| [Later](https://later.com/pricing/), [SocialBee](https://socialbee.com/pricing/) | from $18.75 and $29 a month; no free plan | yes | none mentioned | | same |
| [Repurpose.io](https://repurpose.io/pricing) | $35 a month; 10 free videos to try | yes | none mentioned | drop an MP4 in Google Drive or Dropbox and it fans out | same |
| [Zapier](https://zapier.com/pricing), [Make](https://www.make.com/en/pricing) | free tiers of 100 tasks and 1,000 credits a month | **no native TikTok publishing** on either; Make's TikTok app has ads actions only | | glue only, around Buffer or an aggregator | [Make TikTok app](https://www.make.com/en/integrations/tiktok) |

### What this section decides (D-010 resolved)

- **One master file for every platform:** MP4, H.264 and AAC 48 kHz, 1080x1920, 30 fps, closed GOP, `moov`
  first, 3 to 60 s, under 100 MB. That fits inside every limit above at once.
- **Phase 5, $0 and fully automatic:** direct adapters for Instagram Reels (Instagram Login flavour,
  Standard Access), Facebook Reels (the owner's Page), Threads (tester role) and Bluesky. Each rendered
  file goes to public HTTPS object storage behind a domain the owner controls, because Meta fetches by
  URL and TikTok needs a verified domain later.
- **TikTok:** do not plan on passing the Direct Post audit with a personal tool. Use Buffer's free plan
  (its own approved app, 3 channels, one post a day keeps the queue under ten) as the $0 bridge, or
  upload-post Basic at $24 a month when the budget guard allows. The inbox route is the fallback if a
  daily tap is acceptable.
- **Pinterest and LinkedIn:** phase 5b, after the paperwork (a one-minute OAuth demo video and a Business
  account for Pinterest; the self-serve share product or the Development tier for LinkedIn).
- **X:** skipped until there is a reason; it is cheap but needs a card and has the least reach for
  vertical video.
- **A distribution ledger** in the runner: per-platform daily counters (Instagram 100, Facebook 30,
  Threads 250, TikTok 15, Bluesky 25), a 60-day Meta token refresher, the Threads 30-second wait, and
  every remote post id, so a re-run never double-posts.

**Open questions.** How long a TikTok audit takes and whether a channel with a public website could pass
(third-party claims only); whether the legacy LinkedIn share product is still granted to new apps;
Instagram's 100 versus 50 daily limit; Bluesky's current API-enforced limits (call `getUploadLimits`).

## 7. Orchestration, scheduling and hosting (checked 2026-10-08)

**Summary.** For one video a day the scheduler and host are the cheapest part of the system and can stay
at $0 indefinitely. The strongest free option is a GitHub Actions workflow on a cron trigger: standard
Linux runners are free on public repositories and a private repository gets 2,000 minutes a month on the
Free plan; a job may run six hours; the runner has Python 3.12 and Node 22 and passwordless sudo, so
FFmpeg is one `apt-get` away; `workflow_dispatch` and `issues` events let the owner push an idea from the
GitHub app on a phone. The catches are real but manageable. For GPU bursts, Modal's Starter plan carries
$30 a month of free credit billed per second, and Hugging Face Jobs run scheduled jobs for cents. The
owner's Windows PC is the right development runner and manual backup but a poor production scheduler.

### Free and cheap schedulers

| Option | Cost | What you get | Catches | Source |
|---|---|---|---|---|
| **GitHub Actions** | $0 on a public repo; 2,000 minutes a month on a private repo (Free plan), 500 MB artifact storage; Linux overage $0.006 a minute | cron `schedule` (shortest every 5 minutes), `workflow_dispatch` with up to 25 inputs, `issues` trigger; jobs up to 6 hours; 20 concurrent jobs; secrets store | **no FFmpeg on the image** (install each run or cache a static build); **no GPU** (GPU runners are Team and Enterprise only, $0.052 a minute); cron fires late at the top of the hour under load, so pick an odd minute; public-repo schedules disable after 60 idle days; pushes made with the job's own token do not trigger other workflows (no recursion, but also no "publish on push"); secrets over 48 KB need a workaround | [Billing](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions), [Limits](https://docs.github.com/en/actions/reference/limits), [Events](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows), [GITHUB_TOKEN](https://docs.github.com/en/actions/concepts/security/github_token), [Runner pricing](https://docs.github.com/en/billing/reference/actions-runner-pricing), [Secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) |
| **Windows Task Scheduler** on the owner's PC | $0 | `schtasks /create /sc daily`, triggers on time, logon, idle | a task created with a saved password stops silently when the password changes (`/ru System` avoids it); sleep, hibernate and update reboots skip runs. **Development runner and backup, not the production scheduler.** | [schtasks](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks-create) |
| **n8n** self-hosted | $0 under the Sustainable Use License for personal use (`npx n8n` on Windows with Node 20.19 to 24, or Docker Desktop); n8n Cloud from €20 a month billed annually | visual canvas, community templates; the faceless-Shorts template [#20025](https://n8n.io/workflows/) exists but assumes OpenAI billing and a paid Orshot plan | a second system to maintain if the Python stages stay; hosting for others is not permitted | [Licence](https://github.com/n8n-io/n8n/blob/master/LICENSE.md), [Editions](https://docs.n8n.io/choose-n8n/), [Cloud pricing](https://n8n.io/pricing/) |
| **Make** | Free: 1,000 credits a month, 2 active scenarios, 15-minute interval; Core $12 | a thin publishing tail if a native connector saves work | no TikTok publishing module | [Pricing](https://www.make.com/en/pricing) |
| **Zapier** | Free: 100 tasks a month | | too tight for a daily three-step flow | [Pricing](https://zapier.com/pricing) |

### GPU bursts without owning a GPU

| Option | Price | Notes | Source |
|---|---|---|---|
| **Modal** | Starter plan includes **$30 a month of free credit**; per second: T4 $0.000164, L4 $0.000222, A10 $0.000306, A100 80 GB $0.000694, H100 $0.001097; CPU $0.0000131 a core-second | `modal.Cron` schedules a function (UTC), so Modal can own the whole daily run or just the GPU step; schedules cannot be paused and `Period` resets on redeploy; $30 is about 25 hours of T4 or 5 hours of A100 a month | [Pricing](https://modal.com/pricing), [Cron](https://modal.com/docs/guide/cron) |
| **Hugging Face Jobs** | pay per second for any account with credit: cpu-basic $0.01 an hour, cpu-upgrade $0.03, t4-small $0.40, L4 $0.80, A10G small $1.00, A100 large $2.50 | scheduled jobs with cron syntax, retries, secrets; **default timeout 30 minutes**, so set it | [Jobs overview](https://huggingface.co/docs/hub/jobs-overview), [Jobs guide](https://huggingface.co/docs/huggingface_hub/guides/jobs), [Pricing](https://huggingface.co/pricing) |
| **Hugging Face Spaces** | CPU Basic is free hardware, but **creating a Gradio or Docker Space requires PRO ($9 a month)**; free accounts may host up to 2 ZeroGPU Gradio Spaces | a free-hardware Space sleeps when idle; for batch work use Jobs | [Spaces overview](https://huggingface.co/docs/hub/spaces-overview) |
| **RunPod Serverless** | 24 GB class $0.69 an hour, 4090 $1.10, A100 80 GB $2.72; no free credit | fallback | [Pricing](https://www.runpod.io/pricing) |
| **Colab** | free tier restricted; Pro about $9.99 a month per search snippets (sign-in gated) | **not an automation host**: no remote-control tools, idle timeouts, interactive priority | [FAQ](https://research.google.com/colaboratory/faq.html) |

### Always-on boxes

| Host | Price | Catch | Source |
|---|---|---|---|
| Oracle Cloud Always Free | $0: Ampere A1 up to 2 OCPU and 12 GB (reduced from 4 and 24 in 2026), 200 GB storage | an idle instance (95th-percentile CPU and network under 20 percent for 7 days) may be reclaimed, and a once-a-day job looks idle; A1 capacity is often unavailable in busy regions | [Always Free resources](https://docs.oracle.com/en-us/iaas/Content/FreeTier/freetier_topic-Always_Free_Resources.htm) |
| Hetzner CX23 | about €5.99 a month including the IPv4 address (third-party August 2026 snapshot; Hetzner repriced on 2026-04-01 and its page is JavaScript-only) | confirm in the console | [Overview](https://docs.hetzner.com/cloud/servers/overview/), [Price notice](https://www.hetzner.com/pressroom/statement-price-adjustment/) |
| DigitalOcean | $4 (512 MB) or $6 (1 GB) a month | | [Droplets](https://www.digitalocean.com/pricing/droplets) |
| Fly.io | no free tier; shared-cpu-1x from $2.19 a month, card required | | [Pricing](https://docs.fly.io/about/pricing) |
| Railway, Render | Railway Free is a $1 monthly credit; Render has no free cron jobs ($1 a month minimum per cron service) | | [Railway](https://railway.com/pricing), [Render cron](https://render.com/docs/cronjobs), [Render free](https://render.com/docs/free) |
| Raspberry Pi or mini PC | one-time purchase, not priced here | the same runner as the PC without the sleep problem | |

### State and observability

- **State:** a SQLite file or JSON job files committed back to the repository by the workflow is enough
  while exactly one writer exists; a second runner needs a shared store or a GitHub `concurrency` group.
  [Supabase Free](https://supabase.com/pricing) (500 MB database, 1 GB storage) only if a browsable backlog
  is wanted, and it **pauses after a week of inactivity**.
- **Alerts:** a [Discord webhook](https://docs.discord.com/developers/resources/webhook) (2,000 characters,
  10 embeds, free) or a [Telegram bot](https://core.telegram.org/bots/faq) (sends files up to 50 MB, so
  the rendered short can be reviewed on a phone) per run, plus GitHub's own failure email to the workflow
  author. Keep rendered MP4s as workflow artifacts rather than commits.
- **Guardrails:** GitHub Actions spending limit at $0 (the default), a 25-minute `timeout-minutes` on the
  job, Modal capped at its free credit, an explicit timeout on any Hugging Face Job.

### What this section decides (D-009 revised)

- **Phases 1 and 2** run on the owner's Windows PC from Task Scheduler with `/ru System`, as the
  development loop and manual backup; YouTube's `publishAt` means the PC need not be awake at publish time.
- **From phase 3 the scheduler of record is a GitHub Actions workflow in a private repository** (2,000
  free minutes a month is about 130 ten-minute runs): cron at an odd minute, `workflow_dispatch` with
  idea text and a publish flag, an `issues` trigger for the phone, FFmpeg installed each run, state
  committed back with the job token, a Discord or Telegram message per run.
- **GPU steps** (AI video, local image models) go to Modal under `modal.Cron` inside the $30 credit, or
  to Hugging Face Jobs, or run on the PC's GPU when it is on; they never block the daily post.
- n8n stays optional as a visual front end; Make is acceptable only as a thin tail; Zapier is out.

**Open questions.** Whether commits made by the workflow's own token count as "repository activity" for
the 60-day rule (keep a human-visible journal commit anyway); current Hetzner prices; Docker Desktop on
Windows Home (the requirements page lists Pro, Enterprise and Education); Modal's retry guarantees for
scheduled functions.

## 8. Script generation, quality gates and idea intake (checked 2026-10-08)

**Summary.** Script generation is the cheapest stage by a wide margin: at one to three scripts a day every
hosted model costs cents a month, and three paths cost nothing (Gemini Flash's free tier, OpenRouter's
free models, a local Ollama model on the PC). What the research changed is not the price but the rules:
no provider's JSON mode can enforce a word count, so length is enforced in code and the synthesised audio
is the final arbiter; 130 to 160 words runs slightly long for a 50-second video at the published reading
rate; the quality gates must be ordered cheapest first; and the fabricated-attribution failure needs an
allowlist, not a prompt. For intake, a GitHub issue form is the cleanest single queue, a Telegram bot can
be polled from the PC without a public endpoint, and every push-style source needs a relay.

### Models and cost per script

Assumes about 1,200 input and 700 output tokens for the writer and 1,500 and 200 for the judge.

| Model | Price per million tokens (input, output) | About per video | Notes | Source |
|---|---|---|---|---|
| Claude Haiku 5.5 (`claude-haiku-5-5`) | $0.10, $0.50 for prompts up to 100K tokens | $0.0007 | the cheapest capable writer; batch is half | [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing) |
| Claude Sonnet 5.5 (`claude-sonnet-5-5`) | $2, $10 | $0.014 | a strong judge from a different family than the writer | same |
| Claude Opus 5.5 (`claude-opus-5-5`) | $4, $20 | $0.03 (under $1 a month at one a day) | Anthropic's recommended default; affordable even here | same |
| OpenAI `gpt-5-nano`, `gpt-5-mini` | $0.05, $0.40 and $0.25, $2.00 | $0.0005 and $0.002 | | [OpenAI pricing](https://developers.openai.com/api/docs/pricing) |
| Gemini Flash and Flash-Lite | free tier "free of charge" on the current Flash models; paid 2.5 Flash-Lite $0.10, $0.40 | $0 | free-tier limits are shown only inside AI Studio and free-tier prompts may be used to improve Google's products | [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing), [Rate limits](https://ai.google.dev/gemini-api/docs/rate-limits) |
| OpenRouter `:free` models | $0 | $0 | 50 requests a day until $10 of credit has ever been bought, then 1,000; 16 free models on 2026-10-08; never pin one id | [Limits](https://openrouter.ai/docs/api/reference/limits) |
| Groq | free plan exists; `gpt-oss-20b` $0.075, $0.30 | cents | the Llama models were retired for free and developer tiers on 2026-08-16 | [Models](https://console.groq.com/docs/models), [Deprecations](https://console.groq.com/docs/deprecations) |
| Ollama (local) | $0 | $0 | v0.40.1 (2026-10-07); Windows 10 22H2 or newer, NVIDIA driver 551+; structured outputs via the `format` field | [Releases](https://github.com/ollama/ollama/releases), [Windows](https://docs.ollama.com/windows), [Structured outputs](https://docs.ollama.com/capabilities/structured-outputs) |

Two provider facts that shape the code: Anthropic's structured outputs reject `minLength`, `maxLength`
and numeric constraints and can return non-conforming output on a refusal or `max_tokens` stop
([structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs.md));
OpenAI's require every field to be `required` and every object to carry `additionalProperties: false`
([OpenAI structured outputs](https://developers.openai.com/api/docs/guides/structured-outputs)). So the
schema sent to a model is a relaxed copy of `examples/script-schema.json` without length limits, and the
full schema is validated locally. Claude 4.7 and later tokenise about 30 percent more tokens for the same
text, so estimates built on older counts under-count.

### Length

Published pacing guidance converges on about 2.5 words a second, 140 to 150 words for 60 seconds
([one source](https://breadnbeyond.com/how-many-words-does-a-60-seconds-explainer-video-needs/)). The
earlier target of 130 to 160 words lands at 52 to 64 seconds, so the rules now say **125 to 150 words
for a 50-second video, hook under nine words, measured on the synthesised audio with `ffprobe`**; the
TTS voice's measured words per minute feeds back into the budget.

### Unattended quality gates, cheapest first

1. Schema and stop-reason check (reject a refusal or a truncated reply before parsing).
2. Word count 125 to 150 and hook under nine words; one retry with the measured count in the prompt.
3. A regex banned-claims list: diagnosis, cure, medication, vaccine, invest, stock, crypto, guarantee,
   "God will give you the job". YouTube's monetisation page also bans "AI-generated podcast hosts
   offering financial guidance", so the job pillar never gives financial advice.
4. Attribution gate: a `public_domain` attribution must fuzzy-match the local allowlist (the World
   English Bible file, [Project Gutenberg](https://www.gutenberg.org/policy/permission.html) entries, whose
   quotes need no permission) at 0.95 similarity or better; any other named-person attribution fails.
5. Profanity: [alt-profanity-check](https://pypi.org/pypi/alt-profanity-check/json) 1.9.1 (2026-09-14) in
   Python, with an allowlist for scripture and place names.
6. OpenAI's [moderation endpoint](https://developers.openai.com/api/docs/guides/moderation), which is
   free, for self-harm, harassment and hate flags; a flag sends the job to the review queue.
7. The LLM judge on a different model than the writer, grading one script against a rubric with a
   constrained PASS or FAIL plus 1 to 5 subscores, requiring PASS and originality of 4 or more.
   [Zheng et al. 2023](https://arxiv.org/abs/2306.05685) documents position, verbosity and
   self-enhancement biases in LLM judges, which is why the judge is a different model and never compares
   two scripts side by side.
8. Synthesised audio between 45 and 60 seconds.

Every verdict is logged next to the script id, and a 20-to-30-script eval set with human PASS or FAIL
labels is kept from day one so the judge can be re-run whenever the prompt, model or thresholds change.

### Idea intake for one person

| Source | How it reaches the queue | Notes | Source |
|---|---|---|---|
| **GitHub issue form** (the canonical queue) | a workflow on `issues: opened` parses the body (responses become Markdown under `###` headings), writes the idea file, runs the pipeline, comments the video URL and closes the issue | templates can auto-apply a label; issue-triggered workflows run only from the default branch; the issue body is untrusted and is passed through an environment variable, never inlined into a shell step | [Issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms) |
| **Telegram bot** | the runner long-polls `getUpdates` from the PC, so no public endpoint is needed; it opens the same GitHub issue | polling and webhooks are mutually exclusive; undelivered updates are kept 24 hours | [Bot API](https://core.telegram.org/bots/api) |
| Self-feeding mode | a daily cron job picks the pillar by weekday, asks the writer for five candidate ideas that differ from the last 60, picks one by a diversity score, and opens an issue so it flows through the same gates; a pending human idea takes priority | | |
| Google Sheet | an Apps Script edit trigger posts to the GitHub issues API when a row's status is `ready` | 20,000 URL fetches a day; 6 minutes an execution | [Triggers](https://developers.google.com/apps-script/guides/triggers/installable), [Quotas](https://developers.google.com/apps-script/guides/services/quotas) |
| Notion, Airtable, Tally | push subscriptions need a public HTTPS endpoint (a small relay such as a Cloudflare Worker that calls `repository_dispatch`), or the runner polls the API | Notion subscriptions are created in the UI and need a public endpoint; `repository_dispatch` payloads are capped at 10 top-level properties | [Notion webhooks](https://developers.notion.com/reference/webhooks) |
| Email | the runner polls a Gmail label by IMAP or the Gmail API; push watches must be renewed every 7 days and can drop events | | [Gmail push](https://developers.google.com/workspace/gmail/api/guides/push) |

### What this section decides

- Writer: Claude Haiku 5.5 or Opus 5.5 behind one client interface, with a $0 fallback chain (Gemini
  Flash free tier, OpenRouter free models, Ollama) so a quota error never stops the daily post. Judge: a
  different model family (D-008). A month of scripts costs under $1 on Opus at one video a day and
  about $4 at three a day with a Sonnet judge; the cost sheets in section 10 show the lines.
- The script carries a required `original_angle` field and the generator is given three recent hooks to
  avoid, as the written defence against the inauthentic-content and spam rules.
- Length is 125 to 150 words, enforced in code and by the audio.
- Gates run in the order above; the attribution allowlist and the banned-phrase list live in config.
- The GitHub issue form is the one queue; Telegram and self-feed open issues into it.

**Open questions.** Gemini free-tier daily caps (visible only in AI Studio); Groq free-plan numbers;
local Ollama throughput and structured-output reliability on the owner's hardware; whether a generic
synthetic narration voice needs YouTube's AI-use disclosure (the help page addresses only cloning your
own voice).

## 9. Content strategy and safety: market notes, policies, guidelines (checked 2026-10-08)

The rules this section supports live in `docs/CONTENT_STRATEGY.md`; this is the evidence behind them. One
research pass fetched the pages below on 2026-10-08; where a page refused an automated fetch (Biblica,
HarperCollins, Cambridge, help.instagram.com, most subscriber trackers) the line says "snippet" and the
claim is weaker.

**Summary.** The faith-and-encouragement Shorts niche is large and already faceless: the biggest channels
are a recognisable narrator reading an original script over licensed stock, posted in a fixed daily slot
(morning prayer, night prayer). What YouTube's monetisation policy names as *inauthentic* is exactly the
cheapest version of this pipeline (a quote on a static background with a synthetic voice), so the design
has to make every video distinct on purpose. Audience studies agree on two things and disagree on the
rest: the hook has to land in the first seconds and read with the sound off, and Shorts of 40 seconds or
more earn more engagement from the viewers who stay. The safety picture is consistent across YouTube,
TikTok and Meta: content that promotes or instructs self-harm is removed, recovery and encouragement
content is allowed, and the platforms want crisis resources on the video. Language models fabricate
quotes, so attribution needs an allowlist, and scripture needs a translation whose licence an unattended
pipeline can satisfy, which means a public-domain one.

### The channels that already do this

| Channel | Size | Format | Source |
|---|---|---|---|
| Lion of Judah (`@lionofjudahmotivation`) | 3.52M subscribers, about 21 uploads a month, about 28K views an upload | original narration recorded in-house over stock licensed from Filmpac and Videoblocks; recent titles lean to prophecy and current events | [SponsorRadar, updated 2026-09-29](https://sponsorradar.com/channels/lionofjudahmotivation) (fetched); a mirrored description quoted in a [Substack post](https://truthparadigm.substack.com/p/most-people-dont-even-realize-that) (snippet) |
| Grace For Purpose | about 3.8M subscribers, roughly 1,650 videos, many an hour long | prophecy, motivational prayers, biblical commentary | [vidIQ, data of 2025-02-12](https://vidiq.com/youtube-stats/channel/UCI8gcSTo1FowsRJdilsjsZw) (snippet) |
| Daily Jesus Devotional | about 1.34M subscribers | daily morning-prayer videos | [vidIQ, 2025-09](https://vidiq.com/youtube-stats/channel/UCJd6GPedYU1Muh8zmjMF7tw) (snippet) |

What they share: a voice viewers recognise, a slot viewers expect, and narration written for the video.
Third-party trackers disagree with each other by hundreds of thousands of subscribers and block automated
fetches, so these are orders of magnitude, not measurements. The Lion of Judah name is shared by at least
two large channels, and the motivation channel's prophecy angle draws public "false teacher" criticism
from other Christian creators; borrowing that angle borrows the argument.

### What the audience data says

| Study | Sample | Finding | Caveat |
|---|---|---|---|
| [Adobe Express, 2025-07-01](https://www.adobe.com/express/learn/blog/best-times-to-post-youtube-shorts) | 24,274 Shorts from 280 creators, collected 2025-05-22 | Shorts of 40 seconds or longer were 33 percent more engaging; Tuesday and 4 p.m. best on average, Saturday 4 p.m. the best pair; posts outside 8 to 10 a.m. did better | vendor study; engagement, not retention |
| [Buffer, 2026-07-24](https://buffer.com/resources/best-time-to-post-on-youtube/) | median engagement across 1.8M videos | best Shorts slot Friday 4 p.m., then Friday 6 and 7 p.m.; best days Friday, Saturday, Thursday; times shown in the reader's local zone | vendor study; conflicts with Adobe on the day, agrees on about 4 p.m. |
| [Metricool, 2026-07-28](https://metricool.com/press-release-youtube-study-2026/) | 799,718 videos from 71,177 accounts, February 2025 against February 2026 | the Shorts feed produced 61 percent of YouTube views; Shorts views rose 127 percent in a year while watch time per Short fell to about a third; only 11 percent of accounts under 10,000 subscribers moved up a tier; 83 percent of interactions came in the first 10 days | vendor study; the companion [blog](https://metricool.com/youtube-trends/) puts average Short view time near 16 seconds (snippet) |
| [Sprout Social, 2026-03-31](https://sproutsocial.com/insights/social-scheduling) | about 2 billion engagements, 307,000 profiles | best times for Facebook, Instagram, LinkedIn, Pinterest, TikTok and X; nothing for YouTube | useful only for the cross-posts |

Read together: most viewers leave early and the ones who stay reward length, so the payoff goes in the
first three seconds and the depth after. No platform publishes a "hook within N seconds" rule; every
three-second figure found was a vendor blog, and YouTube's own words are "capture attention" in the first
few seconds ([Shorts tips as reported 2025-04-09](https://www.socialmediatoday.com/news/youtube-shorts-creation-tips/744944/)).
Timing studies conflict, so the scheduler takes them as a seed and switches to the channel's own YouTube
Studio audience data after 30 days.

### Faceless experiments, with numbers

- [Kapwing, 2025-01-06](https://kapwing.com/resources/we-grew-a-faceless-youtube-channel-to-1000-subscribers):
  58 Shorts and 2 long videos made with an AI script, an AI voice and pulled B-roll took 157 days
  (2024-06-17 to 2024-11-21) to reach 1,000 subscribers. Viewers "do not seem to mind AI voices"; a
  repeatable title pattern and search-shaped titles worked; the script generator "hallucinated facts" and
  viewers said so in comments; 500K Shorts views in 90 days and 37 watch hours qualified for nothing; the
  channel was taken down on 2024-10-03 and reinstated within the hour on appeal.
- A [Medium post, 2025-06](https://medium.com/pen-pulse/my-faceless-youtube-channel-just-reached-1-000-subscribers-5e2e0936e8fa)
  (snippet): a faceless Christian channel started 2024-12-31 reached 1,000 subscribers in about six months.
- [Think Media podcast](https://pod.wave.co/podcast/the-think-media-podcast/487-youtube-is-demonetizing-channels-what-you-need-to-know)
  (snippet): a Bible-stories channel with 500,000 subscribers was demonetised with an email citing
  "inauthentic and mass-produced content". Anecdotal; the reasons were not published.
- Every other "faceless Christian channel" case study found was a course or tool sales page with no
  checkable data. Treated as marketing.

### Platform policies that bind the writer

| Policy | What it says | Source |
|---|---|---|
| YouTube suicide and self-harm | removes content that promotes or glorifies suicide, self-harm or eating disorders, gives instructions, or targets minors; allows discussion and recovery stories with context, which "may be restricted if it includes details which may be triggering"; recommends creators include prevention resources in the video and the description; first violation usually a warning, three strikes in 90 days ends the channel | [policy](https://support.google.com/youtube/answer/2802245) (fetched) |
| YouTube crisis resource panels | shown on watch pages for suicide, self-harm and eating-disorder videos and on crisis searches; the US partner is 988; not triggered by watch history; not in every country | [help](https://support.google.com/youtube/answer/10726080) (fetched); search alerts widened to depression, sexual assault, substance abuse and eating disorders on 2021-11-09 per [Social Media Today](https://www.socialmediatoday.com/news/youtube-expands-crisis-response-panels-to-provide-more-mental-health-assist/609772) |
| YouTube medical misinformation | bans "guaranteed cure" claims and content that discourages approved treatment or promotes alternatives in its place; does not name mental-health conditions | [policy](https://support.google.com/youtube/answer/13813322) (fetched) |
| YouTube inauthentic content (YPP) | renamed from "repetitious content" on 2025-07-15; "channels where content feels interchangeable from video to video are not allowed to monetize"; examples include "image slideshows, templated storylines, or scrolling text with minimal or no narrative" and "AI-generated content made with generic or unoriginal templates"; series with a shared intro are fine when each video has its own storyline or focus | [policy](https://support.google.com/youtube/answer/1311392) (fetched); [Plagiarism Today, 2025-07-08](https://www.plagiarismtoday.com/2025/07/08/youtube-targets-inauthentic-content/) |
| YouTube Partner Program thresholds | 1,000 subscribers with 4,000 watch hours in 12 months, or with 10M Shorts views in 90 days; Shorts-feed watch time does not count toward the 4,000; updated terms must be accepted by 2027-01-31 | [YPP](https://support.google.com/youtube/answer/72851) (fetched); detail in `docs/DISTRIBUTION.md` |
| YouTube hashtags and titles | more than 60 hashtags and all are ignored; three are shown by the title; misleading tags can remove the video; titles up to 100 characters; Shorts up to 3 minutes since 2024-10-15 | [hashtags](https://support.google.com/youtube/answer/6390658), [Shorts help](https://support.google.com/youtube/answer/10059070), [YouTube blog 2024-10-03](https://blog.youtube/news-and-events/tall-updates-coming-to-shorts/) (all fetched) |
| TikTok integrity and authenticity, August 2026 version (effective 2026-09-24) | labels are required for "AI-generated or significantly edited content that shows realistic-looking scenes or people" and for audio that mimics a real person's voice; not required for "generic text-to-speech (TTS) narration, when the TTS isn't a recognizable voice of a known individual" or for artistic styles; unlabelled content "may be removed, restricted, or labeled"; self-harm content is removed | [guidelines](https://www.tiktok.com/community-guidelines/en/integrity-authenticity) (fetched through curl; the page renders with JavaScript); auto-labelling through C2PA since 2024-05-09 per [TikTok newsroom](https://newsroom.tiktok.com/en-us/partnering-with-our-industry-to-advance-ai-transparency-and-literacy) |
| Meta suicide, self-injury and eating disorders | removes encouraging content, graphic self-injury and mocking; allows awareness, support and recovery, which may sit behind an 18+ sensitivity screen; directs people who post or search such content to local support; change log last dated 2026-02-27 | [standard](https://transparency.meta.com/policies/community-standards/suicide-self-injury/) (fetched) |
| Meta AI labels | "AI info" label (renamed 2024-07-01) is applied on industry signals or self-disclosure; the Instagram help page requires labelling photorealistic video or realistic-sounding audio, not images, and says "there may be penalties" for not doing so | [Meta newsroom](https://about.fb.com/news/2024/04/metas-approach-to-labeling-ai-generated-content-and-manipulated-media/) (fetched); [Instagram help](https://help.instagram.com/761121959519495) (snippet; the page returned 400 and 403) |
| YouTube altered or synthetic content | disclosure required for meaningfully altered or generated photorealistic content; not required for "cloning one's own voice to create voice overs or dubs", caption creation, idea generation or non-realistic content; repeated non-disclosure can bring labels, removal or YPP suspension; disclosure does not affect monetisation eligibility | [help](https://support.google.com/youtube/answer/14328491) (fetched) |

One rule satisfies all three platforms: set the AI label whenever the visuals are generated, and say
"made with AI tools" in the description. It over-complies on YouTube and TikTok for non-realistic
visuals and is required for realistic ones. A stock-footage video with a generic synthetic voice needs
no label on TikTok and none on YouTube; the pipeline still says so in the description.

### Safe-messaging guidelines

| Guideline | Who it is for | What to take from it | Source |
|---|---|---|---|
| 988 Lifeline, for the press | journalists | always include a referral number and local crisis information; link prevention resources; avoid "splashy headlines"; set a safe-commenting policy; post 988 in the first comment | [page](https://988lifeline.org/professionals/for-the-press/) (fetched); 988 itself: call or text 988, chat at [chat.988lifeline.org](https://988lifeline.org/), 24/7, free, Spanish text and chat ([home page](https://988lifeline.org/), fetched) |
| Recommendations for Reporting on Suicide (SAVE; reportingonsuicide.org redirects here) | journalists | "died by suicide", never "committed"; no method, no note, no "successful" or "failed attempt"; no "epidemic" or "skyrocketing"; report as a public-health issue | [SAVE](https://www.save.org/media/media-recommendations/) and the list on [bethe1to.com](https://bethe1to.com/reporting-on-suicide/) (both fetched) |
| Action Alliance, Framework for Successful Messaging | anyone producing suicide-related public messaging | four parts: strategy, safety, positive narrative, guidelines; the editorial stance is hope, help-seeking, recovery | [site](https://suicidepreventionmessaging.org/) (fetched) |
| Orygen #chatsafe, 2nd edition (2023; US and Canada editions) | young people and, in section 8, influencers who make mental-health content | the only creator-specific guidance found; the PDF was too large to fetch and is copyrighted, so section 8 has to be read by a person and paraphrased into the writer prompt | [announcement](https://orygen.org.au/About/News-And-Events/2023/New-chatsafe-guidelines-help-young-people-and-infl), [guidelines](https://www.orygen.org.au/chatsafe) |
| PlushCare TikTok study | evidence, not guidance | of 500 videos under mental-health advice hashtags in July 2022, physicians rated 83.7 percent misleading and 14.2 percent potentially damaging | [report](https://plushcare.com/blog/tiktok-mental-health/) (fetched; a telehealth vendor, 2022 data, page updated 2025-12-15). Directional evidence for "encouragement, not advice" |

### Quotes: why attribution needs an allowlist

- Language models invent quotes and sources, on the record: NBC New York's I-Team got ChatGPT to produce
  a Bloomberg quote nobody could find and quotes from an unnamed commentator ([2023-02-23](https://www.nbcnewyork.com/investigations/fake-news-chatgpt-has-a-knack-for-making-up-phony-anonymous-sources/4120307/));
  a Cody Enterprise reporter published AI-generated quotes attributed to six people including the
  Wyoming governor ([NBC News, 2024-08-14](https://www.nbcnews.com/news/us-news/wyoming-reporter-caught-using-artificial-intelligence-create-fake-quot-rcna166518));
  the Chicago Sun-Times printed a reading list of books that do not exist ([404 Media, 2025-05-20](https://404media.co/chicago-sun-times-prints-ai-generated-summer-reading-list-with-books-that-dont-exist/)).
- Even famous lines are unsourced: "Be the change you wish to see in the world" traces to Arleen Lorrance
  in 1974, not Gandhi ([Quote Investigator, 2017-10-23](https://quoteinvestigator.com/2017/10/23/be-change/)).
- [Quotable](https://github.com/lukePeavey/quotable) (MIT, 180 requests a minute, 55 open issues) says
  nothing about how its quotes were sourced, so it is not an attribution oracle. Wikiquote separates
  sourced, disputed and misattributed entries and is [CC BY-SA](https://en.wikiquote.org/wiki/Wikiquote:Copyrights),
  so a list copied from it carries attribution.
- Rule, already in `docs/CONTENT_STRATEGY.md`: original lines and public-domain scripture by default; a
  name is attached only when the exact line is on the curated allowlist; otherwise the line runs
  unattributed or is rewritten; every attributed quote is logged with its source.

### Scripture licences

| Translation | Status | Conditions | Source |
|---|---|---|---|
| World English Bible | public domain ("not copyrighted"); the name is a trademark for faithful copies | none; modern English from the 1901 ASV | [worldenglish.bible](https://worldenglish.bible/) (fetched; ebible.org regenerated 2026-10-08) |
| Berean Standard Bible | public domain, CC0 dedication of 2023-04-30 | "all uses are freely permitted"; attribution appreciated, not required | [terms](https://berean.bible/terms.htm) (fetched) |
| King James Version | public domain in the United States; in the United Kingdom the Crown's letters patent have no expiry and printing is licensed to Cambridge, Oxford and Collins; "this royal decree has no effect outside of the UK" | whether a UK-viewable video engages the patent was not established (Cambridge's page refused the fetch) | [Yale guide, 2026-08-10](https://guides.library.yale.edu/newtestament/kjv), [ebible.org](https://ebible.org/kjv/copr.htm) (both fetched) |
| ESV (Crossway) | copyrighted; up to 500 verses without written permission | not more than half of a book or a quarter of the work; the full notice must appear; audio must be verbatim with verbal "ESV" credit; "digital artwork", cards and calendars need written permission; not usable in CC-licensed works; the API is free for non-commercial use | [permissions](https://www.crossway.org/permissions/) (fetched) |
| NIV (Biblica, HarperCollins) | copyrighted; up to 500 verses "in any form (written, visual, electronic or audio)" | not a whole book or a quarter of the work; the full Biblica notice; the gratis grant covers the 2011 edition only and excludes "scripture on a product in which the verse stands alone" | a [verbatim mirror of the notice](https://biblewebapp.com/study/content/texts/ENGNIV/about.html) (fetched) and the [HarperCollins page](https://www.harpercollinschristian.com/permissions/) (snippet; the official pages returned 403) |

A verse card is arguably "digital artwork" under ESV's wording and a "verse standing alone" under NIV's,
which is why `docs/CONTENT_STRATEGY.md` defaults to the World English Bible or the Berean Standard Bible,
keeps the KJV for Psalms and Proverbs cadence, and treats ESV and NIV as opt-in with a verse counter,
the notice in the description and, for ESV, the spoken credit.

### What this section decides

- Positioning is "encouragement, not advice": the writer speaks as a peer, never as a clinician; no
  diagnoses, medication names, cure or "skip treatment" lines, no "just pray harder", no prosperity
  promises; a standing description footer says the video is encouragement, not medical advice.
- A heavy-topic gate: a classifier for suicide, self-harm, eating disorders, abuse, addiction and grief
  routes a script to the heavy template, which runs the safe-messaging lint, requires a help-seeking
  close, adds the crisis block to the description and the pinned first comment, and holds the video in
  `approve` mode. Comments on those videos are held for review on crisis keywords, and the bot never
  replies to one.
- Distinctness is measured, not hoped for: a rotation of at least four visual templates, a distinctness
  score (embedding distance to the last 30 scripts) as a deterministic gate, and the originality score in
  the judge. These join the variety rules already in `docs/CONTENT_STRATEGY.md` section 11.
- Scheduling seeds from Buffer's Friday 4 to 7 p.m. and the 4 p.m. agreement, then follows YouTube
  Studio audience data after 30 days; success is judged on retention, shares and saves, not subscriber
  count, because most small accounts do not move up a tier in a year.
- Series candidates from this pass that `docs/CONTENT_STRATEGY.md` did not already have are added
  there as candidates: letters ("Dear you who got the rejection email today"), permission slips,
  thought rewrites, breathe-with-me, scripture stories in 60 seconds, and call-and-response prayer.

### Open questions

- Does a crisis resource panel or an age restriction reduce a Short's reach? No official statement;
  measure on the channel.
- What does section 8 of #chatsafe prescribe? Read the US-edition PDF by hand.
- Does the KJV Crown patent reach a digital video viewable in the UK? Unresolved.
- First-party subscriber counts for the three channels above; the trackers disagree and block fetches.
- The exact wording and penalties of Instagram's AI-label rule; the help page refused the fetch.
- Whether TikTok or Meta attach crisis resources to videos that merely mention anxiety or depression,
  and whether that affects reach.
- How the inauthentic-content review treats a channel that rotates templates but is wholly
  machine-made; the Think Media case is a podcast summary.
- Figures the researcher remembered but did not verify, so not used anywhere: the Mata v. Avianca
  sanction amount; Motiversity's size; TikTok's upload length limits; a Pray.com "AI Bible" view count.

## 10. Cost sheets for the four concepts (checked 2026-10-08)

Every unit price here was read on the linked page on 2026-10-08 in the section cited; the one price this
section adds (Cloudflare R2) was read the same day. The sheets assume one video a day (30 a month) and show
what changes at three a day (90). One video is a 55-second short with a 140-word script, which is about
700 characters of speech to the voice engine (the sample in `examples/sample-script.json` speaks 156 words
in 782 characters, counting hook, scenes and close), eight visuals, one hook clip where a clip is used, and one upload per platform.
Nothing below includes electricity, the owner's time, or a domain, because the privacy-policy and terms
pages the platform app reviews ask for can sit on GitHub Pages at $0.

### Unit prices by stage

| Stage | $0 option | Paid option and price | Per video (paid) | Per month at 30 | Section |
|---|---|---|---|---|---|
| Script writer | Gemini Flash free tier, OpenRouter `:free` models (50 requests a day), Ollama | Claude Haiku 5.5 about $0.0007 a script; Opus 5.5 about $0.03 | $0.0007 to $0.03 | $0.02 to $0.90 | [8](#8-script-generation-quality-gates-and-idea-intake-checked-2026-10-08), [Anthropic pricing](https://platform.claude.com/docs/en/about-claude/pricing) |
| Judge (different model family) | a local Ollama model | Claude Sonnet 5.5 about $0.014 a judgement | $0.014 | $0.42 | same |
| Voice | Kokoro-82M on the CPU; Google Cloud Text-to-Speech inside its free 1M characters a month (the channel uses about 21,000) | Cartesia Pro $5 a month for 100,000 characters; ElevenLabs Starter $6 for 30,000 credits (about one month at one video a day, no headroom), Creator $22 for 121,000 | | $5 to $22 | [2](#2-text-to-speech-checked-2026-10-08), [Google](https://cloud.google.com/text-to-speech/pricing), [Cartesia](https://cartesia.ai/pricing), [ElevenLabs](https://elevenlabs.io/pricing) |
| Visuals, rungs A and B | brand cards; Pexels and Pixabay | | $0 | $0 | [3](#3-visuals-stock-ai-images-ai-video-checked-2026-10-08) |
| Visuals, rung C (eight AI images) | FLUX.1 schnell locally in ComfyUI, or on a ZeroGPU Space inside five free minutes a day | FLUX.1 schnell on fal at $0.003 a megapixel, rounded up, so about $0.05 for eight portrait images; OpenAI `gpt-image-1-mini` low quality about the same; Ideogram $0.027 to $0.09 an image | $0.05 to $0.72 | $1.50 to $21.60 | [fal](https://fal.ai/models/fal-ai/flux/schnell), [Ideogram on fal](https://fal.ai/models/fal-ai/ideogram/v3) |
| Visuals, rung D, hook clip only (one 8-second clip) | Wan 2.1 1.3B locally on an 8 GB card, slowly | Pika 2.5 at $0.04 a second (720p); Veo 3.1 Lite at $0.05 a second with audio | $0.32 to $0.40 | $9.60 to $12.00 | [Pika](https://api.dev.pika.art/catalog/apis), [Gemini pricing](https://ai.google.dev/gemini-api/docs/pricing) |
| Visuals, rung D, whole video (six to eight clips) | none that is practical | budget models $2.40 to $6.40 a video; Veo 3.1 Standard over $25 | $2.40 to $6.40 | $72 to $192 | section 3 |
| Captions | word timings from Kokoro-FastAPI; `faster-whisper` on the CPU | OpenAI `whisper-1` $0.006 a minute | $0.006 | $0.18 | [4](#4-captions-and-music-checked-2026-10-08), [OpenAI pricing](https://developers.openai.com/api/docs/pricing) |
| Music | Pixabay, Kevin MacLeod with credit, ACE-Step 1.5 locally | Epidemic Sound or Artlist about $10 a month; ElevenLabs Music inside the Creator plan | | $0 to $10 | section 4, [Epidemic](https://www.epidemicsound.com/pricing/), [ElevenLabs Music](https://elevenlabs.io/docs/api-reference/music/compose) |
| Render | FFmpeg | | $0 | $0 | [1](#1-open-source-pipelines-and-render-engines-checked-2026-10-08) |
| Publish, YouTube | Data API: 100 `videos.insert` calls a day and 10,000 units for everything else, per project | | $0 | $0 | [5](#5-youtube-api-and-policy-checked-2026-10-08), [quota](https://developers.google.com/youtube/v3/determine_quota_cost) |
| Publish, other platforms | Instagram, Facebook, Threads and Bluesky direct; Buffer Free for TikTok (3 channels, 10 queued posts a channel) | upload-post Basic $24 a month ($16 on annual billing); Blotato Starter $29 | | $24 to $29 | [6](#6-other-platforms-and-schedulers-checked-2026-10-08), [upload-post](https://www.upload-post.com/llms-full.txt), [Blotato](https://www.blotato.com/pricing), [Buffer](https://buffer.com/pricing) |
| Public URL for platforms that fetch by link | Cloudflare R2 free tier: 10 GB-month of storage, 1M Class A and 10M Class B operations a month, free egress; a month of 60 MB shorts is 1.8 GB | $0.015 a GB-month beyond that | $0 | $0 | [R2 pricing](https://developers.cloudflare.com/r2/pricing/) |
| Tracking | YouTube Analytics API | | $0 | $0 | section 5 |
| Scheduler, CPU | the owner's PC; GitHub Actions on a private repository, 2,000 minutes a month (a 10-minute daily run uses 300) | Linux overage $0.006 a minute; DigitalOcean droplet $4 (512 MB) or $6 (1 GB) a month; n8n Cloud from €20 a month on annual billing | | $0 to $6 | [7](#7-orchestration-scheduling-and-hosting-checked-2026-10-08), [GitHub](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions), [DigitalOcean](https://www.digitalocean.com/pricing/droplets) |
| GPU bursts | Modal's $30 monthly credit (a 90-second L4 step is about $0.02, so a month is under $1 of credit); Hugging Face ZeroGPU, five minutes a day | Hugging Face Jobs L4 $0.80 an hour, about $0.02 a run | $0.02 | $0.60 | [Modal](https://modal.com/pricing), [HF Jobs](https://huggingface.co/docs/hub/jobs-overview) |

A 60-second short encoded at YouTube's recommended 8 Mbps is about 60 MB, which is the figure behind the
artifact and R2 lines (`docs/DISTRIBUTION.md`, encoding row).

### Concept 1, "Zero dollar, home PC"

| Line | Choice | Month |
|---|---|---|
| Writer and judge | free-tier hosted model plus local Ollama | $0 |
| Voice | Kokoro-82M on the CPU | $0 |
| Visuals | rungs A and B | $0 |
| Captions, render | Kokoro timings, FFmpeg | $0 |
| Publish, tracking | YouTube Data and Analytics APIs | $0 |
| Scheduler | Windows Task Scheduler on the PC | $0 |
| **Total** | | **$0** |

Optional lines: the Haiku-plus-Sonnet writer and judge pair adds $0.44 a month, Opus plus Sonnet $1.32.
At three a day nothing changes except the LLM lines, which triple. The cost that is real but not on the
sheet is the PC being on at the scheduled minute.

### Concept 2, "Serverless on GitHub Actions"

| Line | Choice | Month |
|---|---|---|
| Runner | GitHub Actions, private repository: 30 runs of about 10 minutes is 300 of the 2,000 free minutes | $0 |
| Artifacts | 60 MB a run with a 3-day retention is about 180 MB of the 500 MB allowance | $0 |
| Public URL | Cloudflare R2 free tier, 1.8 GB of 10 GB | $0 |
| Writer, judge, voice, visuals, captions, render, publish | as Concept 1; Kokoro and `faster-whisper` run on the runner's CPU | $0 |
| GPU steps when rung C or D is used | Modal cron inside the $30 credit, or a ZeroGPU Space | $0 |
| **Total** | | **$0** |

With the optional paid lines (Opus and Sonnet $1.32, fal FLUX images $1.50) the month is under $3. At
three a day: 900 of 2,000 minutes, artifacts need a 1-day retention or R2 only, and the LLM and image
lines triple to about $8.50. The Actions spending limit stays at $0 so an overage fails the run rather
than billing.

### Concept 3, "Low-code with n8n"

| Line | Choice | Month |
|---|---|---|
| n8n | `npx n8n` on the PC under the Sustainable Use License | $0 |
| or a VPS | DigitalOcean 1 GB droplet | $6 |
| or n8n Cloud | annual billing | about €20 |
| Everything else | the same providers as Concept 1 or 2 behind HTTP nodes | $0 |
| Hosted render API, if the FFmpeg script is replaced | Creatomate, Shotstack or JSON2Video | **not priced in this research** |
| **Total** | | **$0 to $6, plus the render API if one is used** |

The render-API line is the only unpriced item in these sheets; it is priced only if Concept 3 is chosen,
which `docs/PROJECT_PLAN.md` does not recommend.

### Concept 4, "Managed quality stack"

| Line | Low configuration | Month | High configuration | Month |
|---|---|---|---|---|
| Aggregator | upload-post Basic | $24.00 | Blotato Starter | $29.00 |
| Host | DigitalOcean 1 GB droplet | $6.00 | same | $6.00 |
| Voice | Cartesia Pro | $5.00 | ElevenLabs Creator | $22.00 |
| Images (rung C, eight a video) | FLUX.1 schnell on fal | $1.50 | Ideogram on fal | $21.60 |
| Hook clip (one 8-second clip a video) | Pika 2.5 at 720p | $9.60 | Veo 3.1 Lite | $12.00 |
| Music | Pixabay or ACE-Step | $0.00 | Epidemic Sound | $10.00 |
| Writer and judge | Opus 5.5 and Sonnet 5.5 | $1.32 | same | $1.32 |
| Captions, render, YouTube, R2, tracking | as Concept 2 | $0.00 | same | $0.00 |
| **Total at one a day** | | **$47.42** | | **$101.92** |

So the plan's "roughly $50 to $120" holds for a hook clip only. Rung D on the whole video adds $72 to $192
a month on the budget models, which no configuration of D-012's $100 phase-6 cap survives; it stays a
per-series exception. The high configuration is also $1.92 over the cap: the budget guard drops Ideogram
for FLUX first, which saves $20.10.

At three a day: the aggregators are unlimited (Blotato allows 900 TikTok posts a month) and the host is
unchanged; Cartesia's 100,000 characters still cover the 63,000 needed, ElevenLabs Starter does not;
images, hook clips and LLM triple. Low becomes about $72 (24 + 6 + 5 + 4.50 + 28.80 + 0 + 3.96), high about
$172, so three a day on the managed stack is only possible in the low configuration.

### Limits that bind before money does

- YouTube Data API: 100 `videos.insert` calls a day and 10,000 units for everything else, per project
  (section 5). At three a day this is nowhere near binding; the API audit is.
- GitHub Actions: 2,000 minutes a month and 500 MB of artifacts on a private repository; a public
  repository's schedule is disabled after 60 days without activity (section 7).
- Google Cloud Text-to-Speech: 1M characters a month free on current voices (section 2).
- ElevenLabs Starter: 30,000 credits, which one video a day consumes almost exactly.
- Buffer Free: 10 queued posts a channel, so the queue must be fed daily rather than weekly (section 6).
- OpenRouter free models: 50 requests a day until $10 of credit has ever been bought (section 8).
- Hugging Face ZeroGPU: five GPU minutes a day on a free account (section 12).
- Modal: $30 of credit a month, after which GPU steps bill per second (section 7).
- Pexels and Pixabay: rate limits and the 24-hour cache rule in section 3; Pixabay forbids mass downloads.

### What this section decides

- D-012's caps are consistent with the sheets: Concepts 1 and 2 are $0 through phase 4; phase 5 at up to
  $25 buys exactly upload-post Basic ($24, or $16 on annual billing); phase 6 at up to $100 buys Concept 4
  in its low configuration with about $50 of headroom, or the high configuration minus Ideogram.
- The downgrade order the budget guard follows, cheapest saving first: Ideogram to FLUX ($20.10), Epidemic
  to Pixabay ($10), ElevenLabs Creator to Cartesia ($17), Veo Lite to Pika ($2.40) to no hook clip
  ($9.60), Blotato to upload-post ($5) to Buffer Free ($24), VPS to GitHub Actions ($6).
- Three videos a day is affordable only on Concepts 1 and 2 or on Concept 4's low configuration.

## 11. Tools the owner asked about: HeyGen, ChatCut and MiroFish (checked 2026-10-08)

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
  ZeroGPU Gradio Spaces of his own for free (any other Gradio or Docker Space needs PRO), which is a way
  to run Kokoro, Z-Image-Turbo or ACE-Step as a private API without a local GPU; for scheduled batch work
  Hugging Face Jobs (section 7) is the cleaner fit.
- Scripture: the three public-domain translations are downloaded once as parquet files into `assets/`.

**Open questions.** Qwen3-TTS speed on CPU and whether it returns timestamps; whether the LTX community
licence permits this use; ACE-Step 1.5 quality for calm instrumental beds (its benchmark claims are
song-oriented); which Spaces stay up, since a Space is someone's hobby unless it belongs to the model's
authors.

