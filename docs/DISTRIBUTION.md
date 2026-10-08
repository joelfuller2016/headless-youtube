# Distribution — getting the video onto each platform without a human

Everything here was checked against the platform's own documentation on the date shown. These rules
change often; re-check before building on any of them. Scheduler and aggregator options are in the second
half of this document.

## 1. YouTube Shorts (first platform, direct API)

Checked 2026-10-08 against Google's developer and help pages.

### What counts as a Short

- A video uploaded on or after 15 October 2024 with a **square or vertical aspect ratio and a length up
  to three minutes** is categorised as a Short. No hashtag is required.
  Source: [Understand three-minute YouTube Shorts](https://support.google.com/youtube/answer/15424877).
- A Short **over one minute** that carries an active Content ID claim of any kind is **blocked globally**.
  Music from the YouTube Audio Library does not receive Content ID claims. Same source.
- Design consequence: the renderer keeps videos **under 60 seconds** by default so the music choice can
  never block a video, and the same file qualifies as a short on every other platform.

### The upload API

- The YouTube Data API v3 `videos.insert` method uploads a video with title, description, tags, category,
  privacy status, scheduled publish time, made-for-kids flag and an AI-disclosure flag in one call.
  Source: [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert).
- **Quota (changed from the old 1,600-unit model).** Projects now get three buckets per day:
  100 `search.list` calls, **100 `videos.insert` calls**, and 10,000 units for everything else. Each upload
  costs 1 unit from the upload bucket. Quotas reset at midnight Pacific time.
  Sources: [Getting started](https://developers.google.com/youtube/v3/getting-started),
  [Quota costs](https://developers.google.com/youtube/v3/determine_quota_cost). Note: the summary box on
  the quota-cost page still says `videos.insert` costs 1,600 points; the table and the prose below it say
  1 unit in its own bucket. The bucket text is the newer one. One video a day uses 1 percent of the bucket.
- **The private-upload trap.** "All videos uploaded via the `videos.insert` endpoint from unverified API
  projects created after 28 July 2020 will be restricted to private viewing mode. To lift this restriction,
  each API project must undergo an audit to verify compliance with the Terms of Service."
  Source: [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert). The audit is
  requested with the
  [Audit and Quota Extension form](https://developers.google.com/youtube/v3/guides/quota_and_compliance_audits).
  Plan for it: the first videos will land as private until the audit passes, so phase 1 includes
  submitting that form early. A fallback is to upload as private via the API and publish by hand for a
  few weeks, which still saves most of the work.
- **Scheduling.** `status.publishAt` schedules a video; it can only be set when `privacyStatus` is
  `private` and the video has never been published. A past time publishes immediately.
  Source: [Videos resource](https://developers.google.com/youtube/v3/docs/videos).
- **Made for kids.** `status.selfDeclaredMadeForKids` must be set on every upload. This channel is not
  child-directed, so it is `false`. Same source.
- **AI disclosure.** `status.containsSyntheticMedia` lets the uploader disclose realistic altered or
  synthetic content. YouTube's help page says disclosure is required when content makes a real person
  appear to say or do something, alters real footage, generates a realistic scene that did not occur, or
  uses AI music as the main focus. It is **not** required for scripts, captions, or cloning your own voice
  for a voice-over. Sources: [Videos resource](https://developers.google.com/youtube/v3/docs/videos),
  [Disclosing altered or synthetic content](https://support.google.com/youtube/answer/14328491).
  Rule for this project: brand cards, stock clips and clearly stylised AI images do not need the flag;
  photoreal AI video of real-looking scenes does. The metadata stage sets the flag from the render tier.
- **Title** is limited to 100 characters, no `<` or `>`. Same source.
- **File size** up to 256 GB. Irrelevant for shorts but it means no size guard is needed.

### OAuth, and why the token dies after a week

- A Google Cloud project whose OAuth consent screen is **External** and in **Testing** status issues refresh
  tokens that **expire in 7 days**. Source:
  [Using OAuth 2.0](https://developers.google.com/identity/protocols/oauth2) (section on refresh token
  expiration). Fix: set the app's publishing status to **In production**. The `youtube.upload` scope is
  sensitive, so Google shows an "unverified app" warning on the consent screen; for a single-user app that
  is acceptable and the owner clicks through once. The refresh token then lasts until revoked.
- The runner stores the refresh token outside the repo (environment variable, Windows Credential Manager,
  or the CI secret store) and refreshes the access token on every run.

### Monetisation and policy (not a goal, but do not foreclose it)

- **Partner Program thresholds.** Full tier: 1,000 subscribers plus either 4,000 public watch hours in
  12 months **or 10 million Shorts views in 90 days**. Expanded tier (fan funding and Shopping, no ad
  revenue): 500 subscribers, 3 public uploads in 90 days, and either 3,000 watch hours or 3 million Shorts
  views in 90 days. Sources: [YPP overview and eligibility](https://support.google.com/youtube/answer/72851),
  [Expanded YPP](https://support.google.com/youtube/answer/13429240).
- **Inauthentic content (15 July 2025).** YouTube renamed its "repetitious content" policy to "inauthentic
  content" and clarified that it covers content that is "repetitive or mass-produced". Content must "be
  your original creation" and "not be mass-produced, generic, repetitive, or manipulative. It should be
  made for the enjoyment or education of viewers, rather than for the sole purpose of getting views."
  Source: [YouTube channel monetization policies](https://support.google.com/youtube/answer/1311392).
  What it means here: a channel of templated, identical-sounding AI videos is exactly the target. The
  defences are built into the content rules: every script is written to one specific person and moment,
  series rotate, visuals vary, and the quality judge rejects anything generic. See
  `docs/CONTENT_STRATEGY.md`.

### Analytics for the feedback loop

The YouTube Analytics API reports views, average view duration and retention per video for the channel
owner. The track stage pulls these on days 1, 3, 7 and 28. Scope: `yt-analytics.readonly`. Details and
quotas are confirmed in `docs/RESEARCH.md`.

### Uploading without the API

Browser automation (Playwright or Selenium driving YouTube Studio) avoids the audit and quota but breaks
whenever the Studio UI changes and is contrary to YouTube's terms. It is listed as a known option and not
recommended for an unattended system.

## 2. TikTok (direct API is possible, audit required)

Checked 2026-10-08 against TikTok for Developers.

- The Content Posting API offers **Direct Post** (publishes straight to the profile) and **Upload** (sends
  the video to the user's inbox as a draft). Source:
  [Content Posting API, get started](https://developers.tiktok.com/doc/content-posting-api-get-started).
- **"All content posted by unaudited clients will be restricted to private viewing mode."** To lift it,
  the API client must pass an audit. Same source. Same shape as the YouTube trap, so the plan treats TikTok
  as phase 2 and submits the audit as soon as the app works.
- Video limits: MP4 preferred (H.264), up to 4 GB, 23 to 60 fps, 360 to 4096 pixels on each side; all
  creators can post 3-minute videos. Pulling from a URL requires a **verified domain** that the developer
  owns; uploading the file bytes avoids that. Source:
  [Media transfer guide](https://developers.tiktok.com/doc/content-posting-api-media-transfer-guide).

## 3. Instagram Reels and Facebook Reels (direct API, app review and hosting required)

Checked 2026-10-08 against Meta for Developers.

- Reels are published by creating a media container with `media_type=REELS` and a **`video_url` on a public
  server**, then publishing it. The account must be an Instagram professional account; with Facebook Login
  it must be connected to a Facebook Page. Permissions: `instagram_business_content_publish` (Instagram
  Login) or `instagram_content_publish` plus `instagram_basic` and `pages_read_engagement` (Facebook
  Login), with Advanced or Standard Access. Source:
  [Content publishing](https://developers.facebook.com/docs/instagram-platform/content-publishing).
- **100 API-published posts per 24 hours**, checkable via the `content_publishing_limit` endpoint. Same
  source.
- Reels spec: MP4 or MOV, H.264 or HEVC, 23 to 60 fps, max width 1920, 9:16 recommended, AAC audio,
  3 seconds to 15 minutes, 300 MB. Source:
  [IG User Media reference](https://developers.facebook.com/docs/instagram-platform/instagram-graph-api/reference/ig-user/media).
- Design consequence: the pipeline needs somewhere public to host the MP4 for a few minutes (a GitHub
  release asset, an S3 or R2 bucket, or the scheduler's own storage). Facebook Reels use the same public
  URL pattern through the Pages API. Both are phase 2.

## 4. Everything else

Pinterest, X, LinkedIn, Threads, Bluesky and the scheduler and aggregator comparison (self-hosted Postiz
and Mixpost, Buffer, Metricool, Publer, Ayrshare, upload-post, Blotato) are covered with prices and links
in `docs/RESEARCH.md` under *Distribution*, and the choice for phase 2 is recorded in `docs/DECISIONS.md`.

## 5. Per-platform metadata rules the metadata stage applies

| Platform | Title | Description | Hashtags | AI flag | Schedule |
|---|---|---|---|---|---|
| YouTube | ≤100 chars | full description plus crisis resources when flagged | 6 to 10 in description | `containsSyntheticMedia` when render tier is D or photoreal C | `publishAt` with `privacyStatus=private` |
| TikTok | caption only (title field is the caption) | first 100 chars matter | 3 to 5 in caption | TikTok's AI-generated content toggle where the API exposes it | post time chosen by the runner |
| Instagram | none | caption, first line is the hook | up to 10 | Meta's AI label where exposed | container then publish; the runner picks the time |
| Facebook | title | description | 3 to 5 | as Instagram | as Instagram |

## 6. The one-slot-a-day rhythm

The runner publishes at one fixed local time per day per platform. YouTube gets the exact time via
`publishAt`; other platforms are posted when the runner wakes closest to the slot. The slot is config, and
the analytics loop may move it once there is data.
