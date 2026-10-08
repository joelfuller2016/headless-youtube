# Content strategy — what the channel says, and what it must never say

The channel sells nothing but hope and prayer. This document turns that into pillars, repeatable series,
script rules, and safety rules that a machine can follow at 6 a.m. with nobody watching. Market
observations about what is working in this niche, with sources, are in `docs/RESEARCH.md` under
*Content strategy*; this file is the rulebook.

## 1. Who it is for

One tired person on their phone. Not an audience. The script generator is told to write to a specific
person in a specific moment ("you, starting a new job on Monday and sure they will find out you don't
belong"). That is the single biggest defence against sounding like every other AI channel, and it is
also what YouTube's inauthentic-content policy is looking for: content made for the viewer, not for views.

## 2. The six pillars and the rotation

| Pillar | What it covers | Tone rule | Default sensitivity |
|---|---|---|---|
| **Hope** | hard seasons, waiting, starting over, and succeeding in life the slow way: goals, small wins, progress without hustle | gentle, patient, never "everything happens for a reason" | low |
| **Prayer** | short prayers for specific moments | second person, spoken to God on the viewer's behalf, plain words | low |
| **Mental health** | anxiety, depression, burnout, loneliness, grief | never clinical, always "you and a professional", crisis line when heavy | medium or high |
| **Job and career** | interviews, first weeks, layoffs, bad bosses, imposter feelings | practical warmth, one small action | low |
| **Encouragement** | "to the person who..." letters, affirmations | specific, concrete images, no hype | low |
| **Happiness** | gratitude, small joys, rest, relationships | light, slow, never preachy | low |

The self-feeding idea generator rotates through the pillars in that order, one a day (the fixed Sunday and
Monday series in section 8 take precedence), and skips to a
calendar hook when one fits (Monday reset, Friday release, Sunday rest, the first of the month,
World Mental Health Day on 10 October, the week before holidays). Two consecutive days never share a
pillar unless the owner typed the idea.

## 3. Series — repeatable formats with examples

Series make automation easier (fixed structure), branding stronger (viewers recognise them), and the
quality judge stricter (it knows what a good one looks like). Each series names its pillar, its
visual rung, and its target length.

| # | Series id | Pillar | Shape (about 55 s) | Example titles | Rung |
|---|---|---|---|---|---|
| 1 | `a-prayer-for` | prayer | hook names the moment → 4 short petitions → one verse → amen → soft close | "A prayer for your first week at a new job", "A prayer for the 3 a.m. worrier" | A or B |
| 2 | `to-the-person-who` | encouragement | letter read slowly over one long clip, no cuts | "To the person who is tired of being the strong one" | B (letter format) |
| 3 | `one-verse-one-minute` | hope | verse on a card → 30 s reflection → one question to carry | "Psalm 46:10 for the week you cannot fix" | A (scripture card) |
| 4 | `monday-reset` | job-career | 3 small moves for the week, one sentence each | "Three things before your inbox on Monday" | B |
| 5 | `when-your-mind-says` | mental-health | name the lie → say what is true → one grounding step → resources | "When your mind says you're a burden" | B, always `crisis_resources: true` |
| 6 | `small-joys` | happiness | five tiny concrete things to notice today | "Five things that are quietly going right" | B or C |
| 7 | `permission-slip` | encouragement | "You have permission to..." affirmations, kinetic typography | "You have permission to rest before you've earned it" | A (kinetic) |
| 8 | `interview-day` | job-career | a prayer plus one practical tip for a specific work moment | "Before you walk into the interview" | B |
| 9 | `grief-is` | mental-health | gentle, no fixes, one true sentence at a time; always resources | "Grief is love with nowhere to go, and that's allowed" | B, high sensitivity |
| 10 | `night-prayer` | prayer | 45 s (the shortest the voice stage accepts), slower pace, darker palette, for the end of the day | "A prayer before you close your eyes tonight" | A |
| 11 | `you-are-not-behind` | hope | unpicks one comparison trap | "You're not behind, you're on a different page" | B or C |
| 12 | `thank-you-for` | happiness | a short gratitude prayer that names ordinary things | "Thank you for the coffee and the people who stayed" | B |
| 13 | `breathe-with-me` | mental-health | breathing together, a slow shape drawn on screen with one line or one verse per breath, loops cleanly; company, never a technique offered as treatment (YouTube's AI-personas rule) | "Breathe with me before you open that email" | A (breathing overlay) |
| 14 | `sixty-second-story` | hope | a scripture story retold in plain words from the public-domain text, told as a hope story | "Elijah under the broom tree", "Hagar, seen in the desert" | B or C |
| 15 | `pray-with-me` | prayer | call-and-response: the narrator prays a line, the caption invites the viewer to say it; the close asks for an "amen" in the comments | "Pray this with me before your shift" | A or B |
| 16 | `younger-self` | hope | what the narrator would tell their younger self at one specific age, original lines only | "At 25 nobody told me this" | B |

Sixteen series across the four or more visual layouts in section 11 give more combinations than a month
has days, and every video still has a unique person and moment at its centre. Series 13 to 16 were added from the market notes in
`docs/RESEARCH.md` section 9: the call-and-response prayer is the highest-engagement format on the big
prayer channels, and a scripture story needs no modern testimony whose copyright or truth would have to
be checked.

## 4. Script rules (enforced by the generator prompt and the judge)

- **Length.** 125 to 150 spoken words for a 50-second video at about 2.5 words a second, hook under nine
  words. The judge counts, and the synthesised audio (45 to 60 seconds, measured with `ffprobe`) is the
  final arbiter; the voice's measured pace feeds back into the budget.
- **Hook.** The first line names who this is for and the moment. No greeting, no "in this video", no
  question that can be answered "no". It is the first sentence of scene 1, repeated verbatim in the
  `hook` field, and the judge counts it once.
- **Shape.** Hook → three to five scenes of one to three sentences each → close. One idea per scene.
- **Voice.** Second person, present tense, short sentences, concrete nouns. The narrator is a kind
  friend who has been there, not a coach and not a preacher.
- **Faith.** Prayer and scripture appear naturally in the prayer and hope pillars and may appear in
  any other pillar when the idea calls for it. Faith is never a condition ("if you just believed
  more"), never partisan, never a test of the viewer.
- **Close.** A blessing, a question, or "come back tomorrow". Never "like and subscribe", never a sell.
- **Banned.** Clichés ("everything happens for a reason", "God won't give you more than you can handle",
  "good vibes only"), hustle language, shouting, shame, comparisons to other people, miracle promises,
  promised outcomes ("God will give you the job"), any financial advice, and any line in which the
  narrator presents as a professional or offers a technique as a treatment (YouTube's monetisation page
  bars AI personas giving health, legal, financial or political advice, with an AI "doctor" as its first
  example and AI hosts offering financial guidance as its second, `docs/RESEARCH.md` section 9; the job
  pillar is about courage, not money).
- **Original angle.** Every script carries a short `original_angle`, one concrete perspective that no
  recent video used, and the generator is shown the last three hooks to avoid.
- **Originality.** Every sentence written fresh. No quote attributed to any real person unless it comes
  from the verified quote file. No scripture unless it is looked up from the translation file.

## 5. Quotes and scripture — the fabrication problem

Language models invent quotes and misattribute real ones, and a channel that puts a false quote in a
pastor's or a poet's mouth loses trust in one comment. The rules:

1. **Scripture is looked up, not generated.** The pipeline carries a public-domain translation as a
   file and inserts the verse text from it; the model supplies only the reference, as a `{{verse:...}}`
   token in the scene text. bible-api.com serves the same World English Bible text as JSON and is the
   second source the file is checked against (`docs/RESEARCH.md` section 9).
2. **Public-domain translations by default.** The King James Version is public domain outside the
   United Kingdom (the UK holds a perpetual Crown patent; see
   [eBible's KJV note](https://ebible.org/find/details.php?id=eng-kjv)). The
   [World English Bible](https://ebible.org/find/details.php?id=eng-web) is "not copyrighted" and in the
   public domain (the name is a trademark of eBible.org, so unaltered text only). The
   [Berean Standard Bible](https://berean.bible/terms.htm) is dedicated to the public domain under CC0
   as of 30 April 2023, with attribution appreciated but not required. The World English Bible and the
   Berean Standard Bible read naturally aloud; the King James Version is for the lines everyone knows.
3. **Licensed translations only within their gratis terms.** Crossway's
   [ESV permissions](https://www.crossway.org/permissions/) allow up to 500 verses without written
   permission, with the full copyright notice and, for audio and video, a spoken "ESV" credit; the same
   page requires written permission for "digital artwork", cards and calendars, which a verse card
   arguably is. Biblica's and HarperCollins's NIV pages refused automated fetches on 2026-10-08 (HTTP
   403); a verbatim mirror of Biblica's notice allows 500 verses "in any form" with the full notice, and a
   search snippet of the HarperCollins page excludes "scripture on a product in which the verse stands
   alone", so the NIV stays off until the terms are read in a browser. In practice the notice does not
   fit in a 55-second short, so licensed translations stay off by default (decision D-006). Whether the
   King James Version's UK Crown patent reaches a digital video viewable in the UK is unresolved
   (`docs/RESEARCH.md` section 9); the World English Bible and the Berean Standard Bible carry no such
   question, which is one more reason they are the default.
4. **Named-person quotes** come only from a small, hand-verified quote file (public-domain authors,
   with a source line each; [Project Gutenberg](https://www.gutenberg.org/policy/permission.html) texts
   need no permission to quote). Anything else is rewritten as an original line with no attribution.
5. **The attribution gate is code, not a prompt:** a quoted line must match the allowlist at 0.95
   similarity or better, and any other attribution to a named person fails the job. The judge also fails
   any quote it cannot verify, and a failed quote is a `reject`, not a `revise`.

## 6. Mental-health safety rules

Read with YouTube's
[suicide, self-harm and eating disorder policy](https://support.google.com/youtube/answer/2802245), which
says creators should use supportive wording focused on recovery and hope, include prevention resources and
coping strategies in the video and the description, and avoid naming methods or locations.

- **Never**: diagnose, name medications, promise a cure, suggest stopping treatment, suggest faith or
  prayer replaces professional help, describe methods, use graphic detail, say "you'll be fine".
- **Always, for `sensitivity: high`** (suicide, self-harm, eating disorders, abuse, addiction, acute
  grief; the generator sets the field, the judge checks it, and when the gate's keyword list fires,
  including ideation phrases such as "burden" and "better off without me" that never say suicide, the
  gate sets `sensitivity: high` and `crisis_resources: true` itself): speak
  to the viewer as someone worth staying for; include a grounding step; set `crisis_resources: true` so
  the description gets the resource block; use the gentler voice pace and the darker, calmer palette.
- **The resource block** appended by the metadata stage, US first because the owner is in the US:

  > If you're struggling, you can call or text **988** (the 988 Suicide and Crisis Lifeline, free,
  > 24/7, in the United States) or chat at chat.988lifeline.org. Outside the US, findahelpline.com lists
  > local lines. You matter.

  Source for 988: [988lifeline.org](https://988lifeline.org/) (call, text, or chat; 24/7/365).
  [Find A Helpline](https://findahelpline.com/) is run by ThroughLine as a public service and lists
  verified crisis lines by country across more than 175 countries (checked 2026-10-08).
- **Phrase scanner.** A deterministic list of phrases the gate rejects outright regardless of the judge
  (method words, medication names, "cure", "just pray harder"). It lives in config, not in a prompt, so it
  cannot be talked out of.
- **Safe-messaging lint**, from the [Recommendations for Reporting on Suicide](https://www.save.org/media/media-recommendations/)
  and the [988 press guidance](https://988lifeline.org/professionals/for-the-press/): "died by suicide",
  never "committed"; no method, no note, no "successful" or "failed attempt"; no "epidemic" or
  "skyrocketing"; a help-seeking close on every heavy video. The lint rewrites the wording it can and
  rejects the rest.
- **Heavy-topic gate.** A classifier (the generator's `sensitivity` field, checked by the judge and
  against the keyword list above, so it cannot be missed) routes suicide, self-harm, eating disorders, abuse, addiction and acute grief to
  the heavy template: the lint above, the resource block in the description *and* posted as the first
  comment (the Data API cannot pin a comment, so pinning it is the one item on the owner's approval
  checklist, which is what 988 asks the press to do), and `review_mode: approve` for that one video even
  when the channel otherwise runs unattended. Decision D-019.
- **Comment safety.** YouTube's hold-for-review is set once in Studio with a crisis keyword list (there
  is no API for it), the owner sweeps held comments daily, and the bot never replies to a comment that reads as a crisis;
  people answer people.
- **Encouragement, not advice.** The writer is a peer, never a clinician. A 2022 physician review of
  500 TikTok mental-health-advice videos rated 83.7 percent misleading (`docs/RESEARCH.md` section 9); the
  channel's answer is to give no advice at all, only encouragement and the number to call.
- **Mental-health content is "allowed with context" on YouTube**; the channel's context is recovery and
  hope, which is the allowed side of the line. Recovery content can still be age-restricted if it has
  triggering detail, so the rules above keep detail out.

## 7. Titles, descriptions, hashtags

- **Title** (under 70 characters): who it is for plus the moment. "A prayer for your first week at a new
  job." Never clickbait words, never all caps, never emoji in the first 40 characters.
- **Description**: the hook as the first line, two lines on what the video is, the series name, the
  crisis block when flagged, then hashtags. On YouTube the first 100 characters show in feeds.
- **Hashtags**: 3 to 5 everywhere (`#shorts` plus the pillar and series tags on YouTube). YouTube shows
  three by the title and ignores every hashtag on a video that carries more than 60; Instagram's own
  advice is 3 to 5 (sources in `docs/RESEARCH.md` section 9). Lowercase, no spaces. A rotating pool per
  pillar so no two videos carry an identical tag list.
- **Two standing footer lines** in every description: "This is encouragement, not medical or
  mental-health advice." and "Made with AI tools, written for one person at a time."
- **AI disclosure** (decision D-020): the AI label is set on every platform whenever any scene is
  generated (rungs C, D and F, and an E hybrid with a generated hook), which is required for realistic
  content and over-complies for stylised content. For a synthetic voice over stock or a card: YouTube's
  page exempts cloning one's own voice and lists no rule for a generic narrator, so its flag stays off
  and the footer line discloses the voice; TikTok's page exempts generic text-to-speech explicitly; Meta's
  summary names "realistic-sounding audio", so Meta's label is set on every video wherever its API
  exposes it until the help page has been read in a browser. A generated music bed under narration is
  not "the main focus" and sets no flag; a video that is mostly music does. Per-platform flags are in
  `docs/DISTRIBUTION.md`.

## 8. Posting rhythm

- One video a day, same local time, seven days a week. Daily is what the pipeline is for; consistency
  matters more than the slot. The seed slot is 4 p.m. in the owner's time zone, where two vendor studies
  agree (they disagree on the day); after 30 days of data the scheduler follows the channel's own YouTube
  Studio audience hours instead (`docs/RESEARCH.md` section 9).
- Sunday is `night-prayer` or `one-verse-one-minute`. Monday is `monday-reset`. The rest follow the
  rotation.
- After 60 published videos the analytics loop may propose a second daily slot for the strongest pillar;
  the owner decides.

## 9. Voice and brand

- One narrator voice, warm, unhurried, mid-register, chosen once and never changed without a decision.
- Brand kit: one display font and one caption font, both under the SIL Open Font License from Google Fonts
  because the captions are burned into every video (the popular "Hormozi" caption font is personal-use
  only), three palettes (dawn, day, night), a 0.3 s bumper, an
  end card that reads "come back tomorrow", and a licensed music bed with its licence file committed next
  to it. The licence must cover every platform the video goes to, not only YouTube (see
  `docs/VIDEO_CONCEPTS.md`).
- The channel name, handle and avatar are the owner's call and are not decided here.

## 10. What success looks like (so the loop has a target)

- Retention: average view duration above 70 percent on a 55-second video.
- Saves and shares over likes: this content is saved for later and sent to a friend; those are the
  signals the idea generator weights highest once the analytics stage exists.
- Comments that say "I needed this today". Those are read by the owner, not by the machine.
- Expectations, so nobody reads slow growth as failure: in a 2026 study of 799,718 videos only 11 percent
  of accounts under 10,000 subscribers moved up a tier in a year, and 83 percent of a video's
  interactions came in its first 10 days (`docs/RESEARCH.md` section 9). The 7-day and 28-day checks in
  the tracking stage are sized to that.

## 11. Variety rules, because the Spam policy names this exact pipeline

YouTube's Spam policy gives as its example of prohibited mass-production "channels that use the exact
same background music and repetitive AI generated imagery across many videos" with an AI-written
narration in each, and its monetisation policy lists image slideshows, templated storylines and
scrolling text with little narrative as ineligible (sources in `docs/DISTRIBUTION.md`). A channel that
rotates one brand card and one music bed under one script template is that example. These rules are
enforced by the render and gate stages, not by taste:

- **Music.** No two videos in any rolling 14 days share a music bed; the library holds at least 20 beds,
  and generated beds (ACE-Step) get a fresh prompt and seed per video.
- **Visuals.** No stock clip or generated image is reused within 30 days, and the first page of a stock
  search is skipped when a later page fits, because the clips used by thousands of channels both feed the
  inauthentic-content test and draw false Content ID claims from other uploaders; brand cards use at least 12
  backgrounds and 3 palettes and never run two days in a row on the same series.
- **Structure.** Sixteen series with different shapes, on a six-pillar rotation; the judge rejects a
  script whose structure matches the previous day's video.
- **Visual templates.** At least four rotating layouts (calm clip with captions, kinetic typography, the
  breathing overlay, the letter typewriter, verse-and-reflection split, three-step list), so the same
  series does not look the same two days running.
- **Distinctness score.** The gate embeds each script with a small local model and rejects one whose
  nearest neighbour among the last 30 scripts is closer than the threshold set in config; the judge's
  originality score is the second opinion, not the only one (`docs/ARCHITECTURE.md`, gate stage).
- **Narrative.** Every video has a hook, a turn and a close, written to one person. A quote on a
  background with no reflection is not a video; the scripture card always carries the reflection.
- **The owner's perspective.** Each series carries a recurring framing that is Joel's, not the model's:
  a short set of lines, images and stances the owner writes once and the generator is told to draw on,
  so the channel sounds like a person with a point of view. These live in `config/perspective.md`.
- **Volume.** One video a day, at most three, so the channel is a daily word, not a flood.
- **Provenance.** The job file records every asset's source, licence and model, so any question about
  originality can be answered from the record.
