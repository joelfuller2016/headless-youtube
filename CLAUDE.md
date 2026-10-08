# CLAUDE.md — working in `headless-youtube`

## What this repo is

The plan, research, and (later) the code for a fully automated short-form inspirational video channel.
Read `docs/PROJECT_PLAN.md` first, then `docs/ARCHITECTURE.md`. The owner's original idea is kept verbatim
in `docs/BRAIN_DUMP.md`; check design changes against it.

## Journal every piece of work

Every instruction received and every piece of work done gets an entry in `docs/JOURNAL.md`, the repo's one
journal file. Newest entry first, directly under the marker line. One line per entry:
`- YYYY-MM-DD HH:MMZ what happened`, in UTC, naming the commit, PR or issue. Every number and identifier
comes from a command run at the time. A correction is a new entry that says what it corrects.

Commit and push the journal straight to `main` as a fast-forward after each update. It is the only file
that skips the pull-request workflow; everything else goes through a focused PR off a fresh `origin/main`.

## Conventions

- **Never commit secrets.** OAuth tokens, API keys, `client_secret*.json` and `.env` files are ignored by
  `.gitignore` on purpose. Keep it that way.
- **Stage explicitly** (`git add <path>`), never `git add -A`. Rendered media and caches live under
  `output/` and are ignored.
- **Docs carry their own status.** A superseded document gets a banner pointing at what replaced it rather
  than being deleted. Decisions go in `docs/DECISIONS.md` with the date and the reason.
- **Every claim about a price, quota, limit or policy carries a link and the date it was checked.** These
  change often; a number without a date is a guess.
- **Windows-first.** The owner develops on Windows (PowerShell 7 and Git Bash available). Scripts must not
  hard-code a user profile path and should run on Windows without WSL unless the doc says otherwise.
- **Prefer editing over creating.** Do not add a second plan file; change the existing one.
- Conventional commits: `docs:`, `feat:`, `fix:`, `chore:`.

## Layout

| Path | Purpose |
|---|---|
| `docs/PROJECT_PLAN.md` | Vision, goals, principles, concepts, cost tiers, phases |
| `docs/ARCHITECTURE.md` | Pipeline stages, data model, state machine, failure handling |
| `docs/VIDEO_CONCEPTS.md` | The visual format ladder from static image to AI video, with cost and quality |
| `docs/RESEARCH.md` | Tool landscape with links, prices and the date each was checked |
| `docs/DISTRIBUTION.md` | Platform APIs, quotas, policies, schedulers |
| `docs/CONTENT_STRATEGY.md` | Pillars, series formats, script rules, safety rules |
| `docs/ROADMAP.md` | Phases with task checklists |
| `docs/DECISIONS.md` | Decisions made and decisions still open, with recommendations |
| `docs/BRAIN_DUMP.md` | The owner's original idea verbatim, and the questions it raises |
| `docs/JOURNAL.md` | The work journal |
| `examples/` | JSON schemas, sample scripts, and prompt templates referenced by the docs |
