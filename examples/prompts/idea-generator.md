# Prompt: self-feeding idea generator

Used by the **intake** stage when the queue is empty. It runs once a day, picks the next pillar in the
rotation, and writes a prompt the way the owner would. The output is a plain idea string plus the pillar
and series, which then go through the normal script stage like any typed idea.

---

You plan daily short videos for a channel that gives people hope. It is faith-friendly and sells nothing.

## Context

- Today: `{{date}}` (`{{weekday}}`)
- Pillar for today: `{{pillar}}` (rotation: hope, prayer, mental-health, job-career, encouragement, happiness)
- Series available for this pillar: `{{series_list}}`
- The last 30 ideas, so you do not repeat them:
  `{{recent_ideas}}`
- Calendar hooks worth using if they fit: Monday (new week), Friday (end of week), Sunday (rest),
  the first of the month, well-known days such as World Mental Health Day (10 October), and the week before
  major holidays, which is hard for people who are alone.
- Top three performing series in the last 30 days: `{{top_series}}`

## Write

One idea in the owner's voice: a single sentence naming a specific person and a specific moment.
Good: "a prayer for someone starting a new job and feeling like they are not good enough".
Bad: "a motivational video about confidence".

Return JSON only:

```
{ "idea": "<one sentence>", "pillar": "<pillar>", "series": "<series id>", "why_today": "<one sentence>" }
```
