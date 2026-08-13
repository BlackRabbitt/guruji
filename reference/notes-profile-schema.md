# Notes & Profile File-Format Schema

All personal data lives under `data/` (gitignored, see `.gitignore`). This file
defines the on-disk format so any agent session can read/write it consistently.

## Profile — `data/profile.md`

One file, plain markdown with frontmatter. Created only after the user opts in via
the one-time setup offer (see `SKILL.md`). Use `templates/profile-template.md` to
create it — never prefill guessed values, always ask.

```markdown
---
updated: YYYY-MM-DD
---

# Profile

## Background
- Name:
- Career start year:
- Education:
- Current title/role:
- Total years of professional experience:
- Experience summary (domains/complexity/scale, NOT language-by-language years):
  e.g. "8 years total — 4 in healthcare compliance systems, 2 leading a legacy
  monolith-to-services migration, 2 in early-stage product work."
- Languages/frameworks used (context only, not a skill ranking):

## Current Context
- Company/team:
- Primary stack/domain:
- Current project(s):

## Growth Goals
- Target role/level:
- Actively growing:

## Coaching Calibration Notes
Free-form preferences for how the user wants to be talked to (pace, directness,
how much Socratic back-and-forth they generally want, etc.). This is a *starting
prior only* — a live, concept-specific signal in conversation always overrides it,
and it must never be used to lower rigor.
```

Update this file in place (don't create dated copies) whenever the user shares new
durable facts about themselves. Bump `updated` on each change.

## Notes entries — `data/notes/<date>-<slug>.md`

One file per entry. Filename: `YYYY-MM-DD-short-slug.md`. Frontmatter fields:

```markdown
---
date: YYYY-MM-DD
type: decision | mistake | challenge | win | learning
title: Short descriptive title
project: Project or team name
tags: [tag-one, tag-two]
impact: Optional one-line impact statement (e.g. "cut p99 latency 40%")
adr: Optional path/link to a related ADR, if one exists
---

## Situation
What was the context/problem?

## Task
What was the user actually responsible for / trying to achieve?

## Action
What did the user actually do? (Specific, not generic.)

## Result
What happened? Only include outcomes actually discussed/confirmed — never
fabricate metrics or results that weren't stated.

## What I'd Do Differently
Honest reflection, if applicable. "Nothing" is a valid answer if genuinely true.

## Why This Matters
Framed specifically for performance-review and interview usefulness: what does
this demonstrate (ownership, technical depth, judgment under pressure,
cross-team leadership, learning from failure, etc.)? Always fill this in — it's
the field that makes the entry retrievable and usable later.
```

If the entry is about a decision that already has an ADR, link to it in the `adr`
field and keep the entry itself short — don't duplicate the ADR's content, just
summarize the human side (why it mattered, how it landed, what was learned).

## Index — `data/notes/index.md`

A single running table of contents, appended to on each new entry:

```markdown
# Notes Index

| Date | Type | Title | Project | Tags | File |
|---|---|---|---|---|---|
| 2026-01-15 | decision | Chose Postgres over DynamoDB for orders | checkout | [datastore, postgres] | notes/2026-01-15-orders-datastore.md |
```

### Rebuilding the index

If the index is ever suspected stale (e.g. after a restore, or a manual file
edit), rebuild it from scratch by scanning every file in `data/notes/` (excluding
`index.md` itself), reading each file's frontmatter, and regenerating the table
sorted by date ascending. Never trust a possibly-stale index over the actual
entry files — the entries are the source of truth, the index is a derived cache.
