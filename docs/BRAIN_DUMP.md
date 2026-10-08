# Brain dump — the idea in Joel's own words, and what it implies

This file keeps the original idea exactly as it was given, so later design work can always be checked
against the source. Everything below the first section is interpretation and can be argued with.

## 1. The original ask (verbatim, 2026-10-08)

> I need to work on a new project. I've started the headless YouTube channel. My idea is quick short videos
> for inspiration. Not selling anything but hope and prayer.
>
> I need to built out this idea and project plan into a well designed system where I can input an idea or
> prompt for a concept and this proj3ct will explain the idea into a short video created by Ai and then post
> it to social media to help people succeed, not just in life but mental health, job, encouragement,
> happiness.
>
> Help me make this idea come true.
>
> Research online and use the head youtube repo to document your project plan how it can work and bring all
> ideas, this is a brain dump of this
>
> Add links to your resources you use, come up with different concepts ofr a 100% fullt automated process.
> We want free or cheap at 1st but may expand, so think of video creation and quality, but also could be
> voice over with a static image or short background image running with text caption.
>
> See my thoughts around this.
>
> So we will need the ability to cr3ate the content and social media to distribute

## 2. What the ask contains, broken out

- **Mission.** Short videos that give people hope. Faith is welcome ("hope and prayer"), but the audience is
  anyone trying to get through life, mental health struggles, job stress, or just a hard day. Nothing is sold.
- **Input.** One idea or prompt. Examples Joel might type: "a prayer for someone starting a new job",
  "you are not behind in life", "how to get out of bed when depression says no".
- **Output.** A finished short vertical video, made by AI, posted to social media without further work.
- **Automation level.** 100 percent. Once an idea is in, no human step is required. The design should also
  cover the case where no idea is typed and the system feeds itself.
- **Budget.** Free or cheap first. Expand later if it works. So the design must have a true $0 tier and a
  clear path to spend money only where it buys visible quality.
- **Quality ladder.** Joel named the bottom rungs himself: voice-over on a static image, or a short looping
  background clip with text captions. Higher rungs (AI images per scene, AI video clips) are optional upgrades.
- **Two halves.** Content creation and distribution are separate problems and both must be solved.
- **Documentation.** The plan lives in this repo, with links to every resource used, and offers several
  distinct concepts rather than one.

## 3. Questions the ask leaves open (with the assumption made for now)

| Question | Assumption used in the plan | Change it in |
|---|---|---|
| Which platforms beyond YouTube? | YouTube Shorts first; then Instagram, Facebook, Threads and Bluesky directly, TikTok through its inbox route until a paid aggregator, Pinterest deferred (D-010). | `docs/DECISIONS.md` |
| How often? | One video per day to start; the pipeline must not care. | `docs/PROJECT_PLAN.md` |
| Voice: male, female, Joel's own cloned voice? | One consistent AI voice chosen once; cloning is a later option. | `docs/DECISIONS.md` |
| How explicitly Christian? | Faith-forward but welcoming: scripture and prayer appear, never preachy, never partisan. | `docs/CONTENT_STRATEGY.md` |
| Does Joel want to approve videos before they post? | No, by default (100 percent automated): `notify` for the first two weeks (D-011), then `none`; heavy-topic videos always wait for approval (D-019). | `docs/DECISIONS.md` |
| What does "succeed in life" mean here? | Progress and small wins, not money or hustle; it lives in the Hope and job pillars, and financial advice is banned. | `docs/CONTENT_STRATEGY.md` |
| Where does it run? | Joel's Windows PC or GitHub Actions at the $0 tier; a small VPS or n8n later. | `docs/PROJECT_PLAN.md` |
| Monetization? | Not a goal. The plan still avoids anything that would block it later. | `docs/DISTRIBUTION.md` |

## 4. Ideas that grew out of the brain dump

These are additions, not instructions. Each one is cheap to try and easy to drop.

- **Self-feeding idea queue.** A scheduled job picks a content pillar and a theme for the day and writes its
  own prompt, so the channel never goes quiet when Joel is busy.
- **Content pillars as a rotation.** Hope, prayer, mental health, job and career, encouragement, happiness.
  Rotate through them so the channel does not become one-note.
- **Series, not one-offs.** Repeatable formats ("A prayer for...", "To the person who...", "One verse, one
  minute", "Monday reset") are easier to automate, easier to brand, and easier for viewers to recognise.
- **Idea intake from anywhere.** A GitHub issue, a Telegram message, a row in a Google Sheet, or an email all
  land in the same queue.
- **Quality gates without a human.** A second model reviews every script for fabricated quotes, medical
  claims, and tone before anything renders.
- **Feedback loop.** Pull view and retention numbers back from YouTube and let them influence which series
  and hooks get made next.
- **Crisis-aware by design.** Heavy mental-health topics automatically add crisis resources to the
  description and use gentler language rules.
- **Reusable visual kit.** A small set of branded backgrounds, fonts, and a music bed make the $0 tier look
  intentional instead of generic.
- **Repurposing.** The same render goes to every platform; only captions, hashtags, and length rules change.
- **Growth path.** Start with static-image or stock-clip videos, measure, and only then pay for AI images or
  AI video clips on the series that earn it.
