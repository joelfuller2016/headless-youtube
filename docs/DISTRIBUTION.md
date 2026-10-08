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
  A fully decorated upload also spends from the 10,000-unit pool: `thumbnails.set` 50, `captions.insert`
  400 if an SRT is uploaded as well as burned in, `playlistItems.insert` 50, so about 500 units a video
  and room for roughly 20 such videos a day (per-call costs on the quota-cost page). There is no Shorts
  flag on the insert call: a video is a Short by aspect ratio and length, and the hashtag is optional.
- **The private-upload trap.** "All videos uploaded via the `videos.insert` endpoint from unverified API
  projects created after 28 July 2020 will be restricted to private viewing mode. To lift this restriction,
  each API project must undergo an audit to verify compliance with the Terms of Service."
  Source: [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert). The audit is
  requested with the
  [Audit and Quota Extension form](https://developers.google.com/youtube/v3/guides/quota_and_compliance_audits).
  The rule dates from 2020 and nobody has tested it for this project, so phase 2 starts with one real API
  upload to see whether it lands private; the publish stage is built on what that shows. Plan for it: the
  first videos will land as private until the audit passes, so phase 1 includes submitting that form
  early. The audit also expects the Required Minimum Functionality for upload clients (title,
  description and privacy status settable by the user), which the CLI satisfies. A fallback is to upload as private via the API and publish by hand for a
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
- **Put every field in the one insert call.** `videos.update` and `thumbnails.set` each cost 50 units
  from the 10,000-unit general bucket, and an update that omits a property deletes its value. The
  `notifySubscribers` parameter defaults to true. Sources:
  [Quota costs](https://developers.google.com/youtube/v3/determine_quota_cost),
  [Videos: update](https://developers.google.com/youtube/v3/docs/videos/update),
  [Videos: insert](https://developers.google.com/youtube/v3/docs/videos/insert).
- **Thumbnails barely matter for Shorts.** Whether `thumbnails.set` even applies to a Short on a channel
  outside the Partner Program is unverified. Custom Shorts thumbnails opened to Partner Program channels
  first in July 2026 and do not show in the swipe feed; custom thumbnails at all require the
  phone-verified "intermediate" feature level. The pipeline still saves a thumbnail frame but never depends
  on it. Sources: [YouTube blog, 2026-07-24](https://blog.youtube/news-and-events/youtube-studio-custom-thumbnail-updates/),
  [Channel features](https://support.google.com/youtube/answer/9890437).
- **Encoding.** YouTube's recommendation is an MP4 with the `moov` atom at the front, H.264 High
  Profile, closed GOP of half the frame rate, 8 Mbps at 1080p for 24 to 30 fps, AAC-LC 48 kHz 384 kbps
  stereo; there are no Shorts-specific settings. Source:
  [Recommended upload encoding settings](https://support.google.com/youtube/answer/1722171).

### OAuth, and why the token dies after a week

- A Google Cloud project whose OAuth consent screen is **External** and in **Testing** status issues refresh
  tokens that **expire in 7 days**. Source:
  [Using OAuth 2.0](https://developers.google.com/identity/protocols/oauth2) (section on refresh token
  expiration). Fix: set the app's publishing status to **In production**. The `youtube.upload` scope is
  sensitive, so Google shows an "unverified app" warning on the consent screen; for a single-user app that
  is acceptable and the owner clicks through once. The refresh token then lasts until revoked.
- **Service accounts do not work for the Data API** (calls fail with `youtubeSignupRequired`), so the
  only route for an unattended uploader is a user refresh token obtained once in a browser through the
  loopback (localhost) redirect; the out-of-band flow is deprecated. The runner stores that token outside
  the repo (environment variable, Windows Credential Manager, or the CI secret store), refreshes the access
  token on every run, and alerts on `invalid_grant`.
- The [Developer Policies](https://developers.google.com/youtube/terms/developer-policies) (section
  III.I.2) forbid automating uploads "without the user's prior specific and express consent"; here the
  user and the operator are the same person, and that standing consent is recorded in the repo's
  configuration so the record exists if the project is ever audited. The same policies (III.E.4.c) say an
  API client must delete or refresh stored authorised data after 30 days, which is why the track stage
  re-pulls analytics instead of keeping old pulls as the source of truth.
- An unverified app on a sensitive scope shows the "unverified app" warning and is capped at 100 users
  over the project's lifetime; the single owner is one. Refresh tokens also die after six months of no
  use. Sources: [Publishing status](https://support.google.com/cloud/answer/15549945),
  [OAuth verification FAQ](https://support.google.com/cloud/answer/13463817).
- The audit form asks for a website, a privacy policy URL and demo credentials, so a one-page site on a
  domain the owner controls is part of phase 2. Source:
  [Audit and Quota Extension form](https://support.google.com/youtube/contact/yt_api_form). Google's own
  pages give two figures for sensitive-scope verification time, 3 to 5 business days and about 10; carry
  both.
- The Analytics API's default quota is not published; it shows only under APIs and Services, Quotas, in
  the Cloud console for the project. Read it there before sizing the track stage.

### Monetisation and policy (not a goal, but do not foreclose it)

- **Partner Program thresholds.** Full tier: 1,000 subscribers plus either 4,000 public watch hours in
  12 months **or 10 million Shorts views in 90 days**. Expanded tier (fan funding and Shopping, no ad
  revenue): 500 subscribers, 3 public uploads in 90 days, and either 3,000 watch hours or 3 million Shorts
  views in 90 days. Sources: [YPP overview and eligibility](https://support.google.com/youtube/answer/72851),
  [Expanded YPP](https://support.google.com/youtube/answer/13429240).
- **The Spam policy, which carries strikes.** Under "Automated or synthetic mass-production" YouTube's
  Community Guidelines prohibit "using automated tools or AI to churn out high volumes of similar content
  with minimal changes" and give as the example "channels that use the exact same background music and
  repetitive AI generated imagery across many videos" where each video reads an AI-written narration.
  Violations can remove content and bring a warning or a strike; three strikes in 90 days can terminate
  the channel. Source: [Spam, deceptive practices and scams policies](https://support.google.com/youtube/answer/2801973).
  This is the single biggest policy risk to the project, because that example is the naive version of
  this pipeline. The content rules in `docs/CONTENT_STRATEGY.md` section 11 exist to keep the channel on
  the right side of it: no two videos share a music bed and a visual set, structures rotate, and every
  script carries the owner's own perspective.
- **Inauthentic content (15 July 2025).** YouTube renamed its "repetitious content" policy to "inauthentic
  content" and clarified that it covers content that is "repetitive or mass-produced". Content must "be
  your original creation" and "not be mass-produced, generic, repetitive, or manipulative. It should be
  made for the enjoyment or education of viewers, rather than for the sole purpose of getting views."
  The page's examples of ineligible content include "image slideshows, templated storylines, or scrolling
  text with minimal or no narrative, commentary, or educational value" and "AI-generated content made with
  generic or unoriginal templates". Source:
  [YouTube channel monetization policies](https://support.google.com/youtube/answer/1311392).
  What it means here: the brand-card and kinetic-typography formats are the most exposed, so they are
  never the whole channel, and every video must carry narrative and the owner's perspective, not only a
  quote on a background.
- **February 2027 changes.** New Partner Program applicants will need 1,000 subscribers plus 8,000
  qualified watch hours in 365 days or 20 million qualified Shorts views in 90 days; existing members are
  not affected, but earning from the Shorts Creator Pool each month will require 10 million qualified
  Shorts views over the previous 90 days; a channel counts as active with 1,000 watch hours a year, 1
  million Shorts views in 90 days, or two long-form videos or five Shorts every 90 days. Creators keep 45
  percent of their allocated Shorts revenue. Sources:
  [Updates to YPP](https://support.google.com/youtube/answer/12843009),
  [Google blog, 2026-08-11](https://blog.google/intl/en-mena/product-updates/connect-communicate/new-opportunities-to-earn-and-changes-to-the-youtube-partner-program/).
  A daily pipeline clears the activity rule by itself; the view thresholds are far away and not a goal.

### Analytics for the feedback loop

The YouTube Analytics API `reports.query` method returns `views`, `likes`, `averageViewDuration`,
`averageViewPercentage` and `subscribersGained` (among others) and can be filtered and grouped by the
`video` dimension with several video ids at once. Scopes: `yt-analytics.readonly`, and the page notes
requests now also require `youtube.readonly`. The track stage pulls these on days 3, 7 and 28, because day-dimension
reports omit the most recent days, and it prefers `engagedViews` (views past the first frame) to the
public view count, which since 27 August 2026 counts from the first frame on every format. Per-video
retention curves come from the `elapsedVideoTimeRatio` dimension with `audienceWatchRatio`.
Sources: [Reports: query](https://developers.google.com/youtube/analytics/reference/reports/query),
[Metrics](https://developers.google.com/youtube/analytics/metrics),
[Data API revision history](https://developers.google.com/youtube/v3/revision_history), checked 2026-10-08. The Audio Library page also matters here: music downloaded from it "won't be claimed by
a rights holder through the Content ID system" on YouTube, Creative Commons tracks there must be credited
in the description, and YouTube says nothing about use off-platform. Source:
[Audio Library help](https://support.google.com/youtube/answer/3376882).

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
  the API client must pass an audit. Same source. Unaudited clients may also have at most 5 posting users
  per 24 hours, each token is limited to 6 requests a minute, and a creator can post about 15 times a
  day across all clients. The [content sharing guidelines](https://developers.tiktok.com/doc/content-sharing-guidelines)
  list "a utility tool to help upload contents to the account(s) you or your team manages" as
  unacceptable, and [app review](https://developers.tiktok.com/doc/app-review-guidelines) requires a
  demo video and a public website and says apps "must not be for private or personal use". So a
  personal pipeline should not expect to pass the audit. The Upload (inbox) route needs no audit, but the
  creator must open the inbox notification and finish the post by hand
  ([Upload guide](https://developers.tiktok.com/doc/content-posting-api-get-started-upload-content)).
  Decision D-010: TikTok goes through Buffer's free plan or a paid aggregator with its own approved app.
- Video limits: MP4 preferred (H.264), up to 4 GB, 23 to 60 fps, 360 to 4096 pixels on each side; all
  creators can post 3-minute videos. Pulling from a URL requires a **verified domain** that the developer
  owns; uploading the file bytes avoids that. Source:
  [Media transfer guide](https://developers.tiktok.com/doc/content-posting-api-media-transfer-guide).

## 3. Instagram Reels and Facebook Reels (direct API, app review and hosting required)

Checked 2026-10-08 against Meta for Developers.

- Reels are published by creating a media container with `media_type=REELS` and a **`video_url` on a public
  server**, then publishing it. The account must be an Instagram professional account. With the
  **Instagram Login** flavour of the API no Facebook Page is needed and **Standard Access is enough when
  the app only serves your own professional account**, so no App Review; with Facebook Login the account
  must be connected to a Page. Permissions: `instagram_business_basic` and
  `instagram_business_content_publish` (Instagram Login) or `instagram_content_publish` plus
  `instagram_basic` and `pages_read_engagement` (Facebook Login). Long-lived tokens last 60 days, so the
  runner refreshes them. Sources:
  [Content publishing](https://developers.facebook.com/docs/instagram-platform/content-publishing),
  [Instagram Login](https://developers.facebook.com/docs/instagram-platform/instagram-api-with-instagram-login),
  [Overview](https://developers.facebook.com/docs/instagram-platform/overview).
- **100 API-published posts per 24 hours**, checkable via the `content_publishing_limit` endpoint. Same
  source.
- Reels spec: MP4 or MOV, H.264 or HEVC, 23 to 60 fps, max width 1920, 9:16 recommended, AAC audio,
  3 seconds to 15 minutes, 300 MB. Source:
  [IG User Media reference](https://developers.facebook.com/docs/instagram-platform/instagram-graph-api/reference/ig-user/media).
- Facebook Reels publish to a Page with `pages_manage_posts`, 30 API-published Reels per 24 hours,
  **3 to 90 seconds**, 1080x1920 recommended; a hosted `file_url` must allow the `facebookexternalhit/1.1`
  user agent. Source: [Reels publishing](https://developers.facebook.com/docs/video-api/guides/reels-publishing).
- Threads publishes from the same Meta app once the owner is added as a Threads Tester, with no App
  Review, 250 posts per 24 hours, videos up to 300 seconds fetched from a public URL. Source:
  [Threads posts](https://developers.facebook.com/docs/threads/posts).
- Design consequence: the pipeline needs public HTTPS object storage behind a domain the owner controls
  (Cloudflare R2 or Azure Blob with a custom domain), because Meta fetches by URL and TikTok's pull
  route needs a verified domain. All of this is phase 5.

## 4. Everything else

Bluesky needs no registration (MP4 up to 300 MB, 25 videos a day at launch). Pinterest's trial tier
makes Pins visible only to their creator until a Standard upgrade that requires a demo video and a
Business account. X is pay-per-use at $0.015 a post with a card on file. LinkedIn is free but gated by
tiers. The scheduler and aggregator comparison (Postiz, Mixpost, Buffer, upload-post, Blotato, Ayrshare,
Publer, Metricool, Later, SocialBee, Repurpose, Zapier, Make) is in `docs/RESEARCH.md` section 6, and
the choice is D-010 in `docs/DECISIONS.md`: direct adapters for the Meta surfaces and Bluesky, Buffer's
free plan as the TikTok bridge.

## 5. Per-platform metadata rules the metadata stage applies

| Platform | Title | Description | Hashtags | AI flag | Schedule |
|---|---|---|---|---|---|
| YouTube | ≤100 chars | full description plus crisis resources when flagged, and the same block as a pinned first comment on heavy videos | 3 to 5 in description (YouTube shows three and ignores all of them past 60) | `containsSyntheticMedia` when render tier is D or photoreal C; YouTube's exemption list covers a synthetic voice over stock | `publishAt` with `privacyStatus=private` |
| TikTok | caption only (title field is the caption) | first 100 chars matter | 3 to 5 in caption | TikTok's AI-generated content toggle where the API exposes it; the August 2026 guidelines require it for realistic AI scenes and exempt generic text-to-speech narration (`docs/RESEARCH.md` section 9) | post time chosen by the runner |
| Instagram | none | caption, first line is the hook | 3 to 5 (Instagram's own advice) | Meta's AI label where exposed; required for photorealistic video or realistic-sounding audio, with penalties stated for not labelling | container then publish; the runner picks the time |
| Facebook | title | description | 3 to 5 | as Instagram | as Instagram |

Generated media keeps its provenance: Google's images and Veo clips carry a SynthID watermark, several
providers attach C2PA metadata, and TikTok has auto-labelled uploads that carry it since 2024-05-09. The
pipeline does not strip any of it (stripping may breach the provider's terms) and expects an automatic
"AI" label on those uploads whether or not it set the flag itself.

## 6. The one-slot-a-day rhythm

The runner publishes at one fixed local time per day per platform. YouTube gets the exact time via
`publishAt`; other platforms are posted when the runner wakes closest to the slot. The slot is config, and
the analytics loop may move it once there is data.
