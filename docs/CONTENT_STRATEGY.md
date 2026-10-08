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
| **Hope** | hard seasons, waiting, starting over | gentle, patient, never "everything happens for a reason" | low |
| **Prayer** | short prayers for specific moments | second person, spoken to God on the viewer's behalf, plain words | low |
| **Mental health** | anxiety, depression, burnout, loneliness, grief | never clinical, always "you and a professional", crisis line when heavy | medium or high |
| **Job and career** | interviews, first weeks, layoffs, bad bosses, imposter feelings | practical warmth, one small action | low |
| **Encouragement** | "to the person who..." letters, affirmations | specific, concrete images, no hype | low |
| **Happiness** | gratitude, small joys, rest, relationships | light, slow, never preachy | low |

The self-feeding idea generator rotates through the pillars in that order, one a day, and skips to a
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
| 5 | `when-your-mind-says` | mental-health | name the lie → say what is true → one grounding step → resources | "When your mind says you're a burden" | B |
| 6 | `small-joys` | happiness | five tiny concrete things to notice today | "Five things that are quietly going right" | B or C |
| 7 | `permission-slip` | encouragement | "You have permission to..." affirmations, kinetic typography | "You have permission to rest before you've earned it" | A (kinetic) |
| 8 | `interview-day` | job-career | a prayer plus one practical tip for a specific work moment | "Before you walk into the interview" | B |
| 9 | `grief-is` | mental-health | gentle, no fixes, one true sentence at a time; always resources | "Grief is love with nowhere to go, and that's allowed" | B, high sensitivity |
| 10 | `night-prayer` | prayer | 40 s, slower pace, darker palette, for the end of the day | "A prayer before you close your eyes tonight" | A |
| 11 | `you-are-not-behind` | hope | unpicks one comparison trap | "You're not behind, you're on a different page" | B or C |
| 12 | `thank-you-for` | happiness | a short gratitude prayer that names ordinary things | "Thank you for the coffee and the people who stayed" | B |

Twelve series on a six-pillar rotation gives roughly 60 distinct videos a month before any series repeats
a shape with the same pillar, and every video still has a unique person and moment at its centre.

## 4. Script rules (enforced by the generator prompt and the judge)

- **Length.** 130 to 160 spoken words for a 50 to 58 second video at a slow pace (about 2.5 words a
  second). The judge counts.
- **Hook.** The first line names who this is for and the moment. No greeting, no "in this video", no
  question that can be answered "no".
- **Shape.** Hook → three to five scenes of one to three sentences each → close. One idea per scene.
- **Voice.** Second person, present tense, short sentences, concrete nouns. The narrator is a kind
  friend who has been there, not a coach and not a preacher.
- **Faith.** Prayer and scripture appear naturally in the prayer and hope pillars and may appear in
  any other pillar when the idea calls for it. Faith is never a condition ("if you just believed
  more"), never partisan, never a test of the viewer.
- **Close.** A blessing, a question, or "come back tomorrow". Never "like and subscribe", never a sell.
- **Banned.** Clichés ("everything happens for a reason", "God won't give you more than you can handle",
  "good vibes only"), hustle language, shouting, shame, comparisons to other people, miracle promises.
- **Originality.** Every sentence written fresh. No quote attributed to any real person unless it comes
  from the verified quote file. No scripture unless it is looked up from the translation file.

## 5. Quotes and scripture — the fabrication problem

Language models invent quotes and misattribute real ones, and a channel that puts a false quote in a
pastor's or a poet's mouth loses trust in one comment. The rules:

1. **Scripture is looked up, not generated.** The pipeline carries a public-domain translation as a
   file and inserts the verse text from it; the model supplies only the reference.
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
   permission, with the full copyright notice and, for audio and video, a spoken "ESV" credit. Biblica's
   NIV permissions page could not be fetched on 2026-10-08 (HTTP 403), so the NIV is not used until its
   terms are read. In practice the notice does not fit in a 55-second short, so licensed translations
   stay off by default (decision D-006).
4. **Named-person quotes** come only from a small, hand-verified quote file (public-domain authors,
   with a source line each). Anything else is rewritten as an original line with no attribution.
5. **The judge fails any quote it cannot verify**, and a failed quote is a `reject`, not a `revise`.

## 6. Mental-health safety rules

Read with YouTube's
[suicide, self-harm and eating disorder policy](https://support.google.com/youtube/answer/2802245), which
says creators should use supportive wording focused on recovery and hope, include prevention resources and
coping strategies in the video and the description, and avoid naming methods or locations.

- **Never**: diagnose, name medications, promise a cure, suggest stopping treatment, suggest faith or
  prayer replaces professional help, describe methods, use graphic detail, say "you'll be fine".
- **Always, for `sensitivity: high`** (suicide, self-harm, abuse, eating disorders, acute grief): speak
  to the viewer as someone worth staying for; include a grounding step; set `crisis_resources: true` so
  the description gets the resource block; use the gentler voice pace and the darker, calmer palette.
- **The resource block** appended by the metadata stage, US first because the owner is in the US:

  > If you're struggling, you can call or text **988** (the 988 Suicide and Crisis Lifeline, free,
  > 24/7, in the United States) or chat at chat.988lifeline.org. Outside the US, findahelpline.com lists
  > local lines. You matter.

  Source for 988: [988lifeline.org](https://988lifeline.org/) (call, text, or chat; 24/7/365).
  The international directory link is to be verified before phase 3 ships.
- **Phrase scanner.** A deterministic list of phrases the gate rejects outright regardless of the judge
  (method words, medication names, "cure", "just pray harder"). It lives in config, not in a prompt, so it
  cannot be talked out of.
- **Mental-health content is "allowed with context" on YouTube**; the channel's context is recovery and
  hope, which is the allowed side of the line. Recovery content can still be age-restricted if it has
  triggering detail, so the rules above keep detail out.

## 7. Titles, descriptions, hashtags

- **Title** (under 70 characters): who it is for plus the moment. "A prayer for your first week at a new
  job." Never clickbait words, never all caps, never emoji in the first 40 characters.
- **Description**: the hook as the first line, two lines on what the video is, the series name, the
  crisis block when flagged, then hashtags. On YouTube the first 100 characters show in feeds.
- **Hashtags**: 6 to 10 on YouTube (`#shorts` plus the pillar and series tags), 3 to 5 on TikTok and
  Facebook, up to 10 on Instagram. Lowercase, no spaces. A rotating pool per pillar so no two videos
  carry an identical tag list.
- **AI disclosure**: set per platform from the render tier, see `docs/DISTRIBUTION.md`. The description
  never hides that the voice is synthetic if asked; the channel description says the videos are made with
  AI tools and written for one person at a time.

## 8. Posting rhythm

- One video a day, same local time, seven days a week. Daily is what the pipeline is for; consistency
  matters more than the slot.
- Sunday is `night-prayer` or `one-verse-one-minute`. Monday is `monday-reset`. The rest follow the
  rotation.
- After 60 published videos the analytics loop may propose a second daily slot for the strongest pillar;
  the owner decides.

## 9. Voice and brand

- One narrator voice, warm, unhurried, mid-register, chosen once and never changed without a decision.
- Brand kit: one display font and one caption font, three palettes (dawn, day, night), a 0.3 s bumper, an
  end card that reads "come back tomorrow", and a licensed music bed with its licence file committed next
  to it.
- The channel name, handle and avatar are the owner's call and are not decided here.

## 10. What success looks like (so the loop has a target)

- Retention: average view duration above 70 percent on a 55-second video.
- Saves and shares over likes: this content is saved for later and sent to a friend; those are the
  signals the idea generator weights highest once the analytics stage exists.
- Comments that say "I needed this today". Those are read by the owner, not by the machine.
