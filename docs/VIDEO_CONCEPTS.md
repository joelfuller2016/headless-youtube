# Video concepts — the format ladder from free to premium

Joel's brief names the bottom rungs himself: "voice over with a static image or short background image
running with text caption." This document turns that into a ladder of five formats that share one
pipeline, so the channel can start at the free end and climb one rung at a time, per series, only where
the numbers justify it. Prices are summarised here and sourced with dates in `docs/RESEARCH.md`.

## 0. What every rung shares

- Vertical 1080x1920, 30 fps, under 60 seconds.
- One warm narrator voice, slow pace, the same voice on every video.
- Large animated captions, one to three words at a time, with emphasis words in the brand colour. On a
  phone with the sound off, the captions *are* the video.
- A licensed music bed, ducked under the voice, from a curated local library of 20 to 30 tracks: the
  YouTube Audio Library's attribution-free tracks (the only music YouTube itself says will not be claimed)
  and Pixabay music (free, no attribution, covers every platform, but contributors can register tracks in
  Content ID, so the licence summary is stored per track for disputes). Fewer distinct tracks means fewer
  surprise claims. A $10-a-month safelisting subscription replaces this in phase 5. Sources and the
  claim-check step in `docs/RESEARCH.md` section 4 and `docs/ARCHITECTURE.md`.
- A 0.3 second brand bumper at the start and an end card that says only "come back tomorrow".
- The hook scene is the only place the pipeline spends extra; everything after it can be plain.

## 1. The ladder

### Rung A — Brand card (cost: $0)

**What the viewer sees.** A branded background (soft gradient, paper texture, or one of about a dozen
owned photographs) with the captions as the main visual, a slow drift on the background so it is never a
still frame, and the scripture reference or series name small at the top.

**Why it works.** Quote-card and "text on calm background" shorts are an established format in the
encouragement niche, and viewers judge them on the words and the voice, not on footage. It is also the
only rung with zero external dependencies, which makes it the fallback for every other rung.

**How it is made.** Pillow or FFmpeg's `drawtext`/`zoompan` filters render the background; the ASS
caption file is burned in. Twelve backgrounds and three colour themes are enough for a month without
visible repetition.

**Risks.** Looks like a thousand other channels unless the typography and the voice are distinctive.
The quality judge's originality check and a consistent brand kit are the mitigations.

### Rung B — Stock clip loop (cost: $0)

**What the viewer sees.** A calm, human, real-world clip per scene (a window at dawn, hands on a mug,
a train at sunset), cropped to 9:16, with a slow push, and the captions over it.

**Why it works.** Real footage reads as sincere, which matters for prayer and mental-health topics where
AI imagery can feel uncanny. It also avoids any AI-disclosure question.

**How it is made.** The scene's visual prompt becomes a Pexels video search (`orientation=portrait`);
the best match is downloaded, trimmed to the scene length and colour-graded to the brand look. Pexels
allows free use and modification with no attribution required under its licence, and the API allows
200 requests an hour and 20,000 a month by default with a request that the app credit Pexels
([licence](https://www.pexels.com/license/), [API](https://www.pexels.com/api/documentation/)).
Pixabay is the second source. Downloaded clips are cached by search term so the same clip is never
fetched twice and the monthly budget is never touched.

**Risks.** Stock clips repeat across channels, and a bad search match (a smiling office stock clip under
a grief prayer) is worse than a brand card. The mitigation is a curated allow-list of search terms per
mood and a fallback to Rung A when the search returns nothing with the right mood.

### Rung C — AI image with motion (cost: two to thirty cents per video)

**What the viewer sees.** One generated image per scene in a consistent painterly or soft-photographic
style (never photoreal people), with a Ken Burns move, and the captions over it.

**Why it works.** Every scene matches the words exactly, the look is ownable, and the cost is a few
cents per video through an API or zero on a local GPU.

**How it is made.** The scene's visual prompt plus a fixed style suffix goes to an image API
(FLUX.1 schnell through fal.ai at $0.003 a megapixel, OpenAI's mini image model at about $0.006 a
portrait frame, or Google's Nano Banana at about $0.034) or to a local ComfyUI install on a Windows GPU,
where the Apache-licensed FLUX.1 schnell weights run on a 4 GB card and Z-Image-Turbo (Apache-2.0, better
text rendering) on a 16 GB card, both for nothing. Without a local GPU, a Hugging Face ZeroGPU Space
renders a short's images inside the five free minutes a day (`docs/RESEARCH.md` section 12). The image is generated at 1080x1920 or upscaled, and
FFmpeg's `zoompan` adds the move. Images are cached by prompt hash.

**Risks.** Hands, text and faces still go wrong; a stylised, people-light look avoids most of it. A
photoreal scene that "did not occur" would need YouTube's synthetic-content flag; a clearly stylised
image does not. See `docs/DISTRIBUTION.md`.

### Rung D — AI video clips (cost: about $2.40 to $6.40 per video on budget models)

**What the viewer sees.** Generated 5 to 8 second clips per scene, in motion, with captions.

**Why it works.** The most "produced" feel and the strongest hook, if the clips are good.

**How it is made.** The scene prompt goes to a video API (Google Veo 3.1 Lite at $0.05 a second, Pika 2.5
at $0.04, Kling, Runway, Luma, Hailuo, or an open-weight model such as Wan hosted on fal). Clips are
generated vertical where the provider supports it, audio off, and preferably from the rung-C still
(image-to-video) so a failed clip falls back to the identical Ken Burns image. Six to eight clips per
video. No video API has a free tier. Prices with dates in `docs/RESEARCH.md` section 3.

**Risks.** The expensive rung, with the most failures (bad motion, uncanny faces, provider queues) and
the clearest disclosure obligation. Reserve it for the hook scene of top-performing series, and never
let a job depend on it: a failed clip drops the scene to Rung C.

### Optional rung F — AI presenter (cost: about $1 to $4 per video, deferred)

A lip-synced AI presenter (HeyGen or similar) reads the script to camera. It is the one rung that puts a
face on a faceless channel, so it is not part of the launch plan. It may suit one or two series where a
person speaking to camera beats b-roll; it always needs YouTube's synthetic-media disclosure, and the
uncanny-valley risk is highest on prayer and grief topics. See `docs/RESEARCH.md` section 11 and D-016.

### Rung E — Hybrid (cost: pennies to dimes)

Rung D or C for the hook scene, Rung B elsewhere, Rung A for the scripture or closing card. This is the
expected steady state once the channel has data on which series earn the spend.

## 2. Comparison

| | A brand card | B stock loop | C AI image | D AI video | E hybrid |
|---|---|---|---|---|---|
| Marginal cost per video | $0 | $0 | about $0.02 to $0.30 for eight images | about $2.40 to $6.40 on budget models, $25 on the best | pennies to a dollar |
| External dependency | none | stock API | image API or GPU | video API | mixed |
| Render time on a laptop | under 1 min | 1 to 2 min | 2 to 5 min | provider-bound | mixed |
| Disclosure needed | no | no | only if photoreal | usually yes | depends on hook |
| Failure modes | looks generic | wrong clip mood | hands, text, faces | everything above plus queues | mixed |
| Best for | scripture, prayer | mental health, encouragement | hope, happiness | hooks only | steady state |

## 3. Three "formats within a format" that cost nothing extra

- **Kinetic typography.** No background at all: words animate in and out on a solid brand colour. Pure
  captions, highest contrast, works at Rung A. Good for affirmations.
- **Letter format.** "To the person who..." read slowly over a single long clip with no cuts. One stock
  clip, one voice, no scene changes. Calm and cheap.
- **Scripture card.** One verse, one minute: the verse shown in full on a brand card while the narrator
  reads it and offers a 30-second reflection. Public-domain translations only, see
  `docs/CONTENT_STRATEGY.md`.

## 4. Technical notes that apply to every rung

- The same ASS caption file and the same audio mix are used by every rung, so switching rungs changes
  only the background layer.
- Loudness: normalise the mix to about -14 LUFS integrated so the channel sounds consistent.
- Safe area: keep captions out of the top 15 percent and bottom 20 percent of the frame, where platform
  UI sits.
- Thumbnails: the hook frame with the title in the brand font. Most short feeds ignore it; it is still
  generated because YouTube uses it in some surfaces and it costs nothing.
- Everything renders with FFmpeg. Remotion is an optional upgrade for animated captions; it is free for
  individuals and companies with up to three employees under its own licence
  ([LICENSE.md](https://github.com/remotion-dev/remotion/blob/main/LICENSE.md)).

## 5. How to climb

1. Launch on Rung A and B together (brand cards for scripture and prayer, stock loops for the rest).
2. After 30 published videos, compare retention per series.
3. Turn on Rung C for the two best series, measure for 30 days.
4. Only then try Rung D, and only for hook scenes.
5. Keep the downgrade ladder on at every step so no provider outage ever stops a daily post.
