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

## Logging Mode
- mode: L1 EXPLICIT | L2 AUTO
```

`Logging Mode` controls whether progress notes are written only on request
(L1, the default when the section is missing) or automatically in the
background with a bold announcement per write (L2). Behavior is defined in
`SKILL.md` under "Progress notes".

Update this file in place (don't create dated copies) whenever the user shares new
durable facts about themselves. Bump `updated` on each change.

## Notes entries — `data/notes/<date>-<slug>.md`

One file per entry. Filename: `YYYY-MM-DD-short-slug.md`. Frontmatter fields:

```markdown
---
date: YYYY-MM-DD
updated: Optional YYYY-MM-DD, set when the entry is revised on a later day
type: decision | mistake | challenge | win | learning
title: Short descriptive title
project: Project or team name
tags: [tag-one, tag-two]
impact: Optional one-line impact statement (e.g. "cut p99 latency 40%")
adr: Optional path/link to a related ADR, if one exists
pii: Optional list of flagged PII values with their kind, e.g. [name: Jane]. Whether raw PII may appear at all follows the active P level (see reference/safety-policies.md, "PII in progress notes")
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

A single running table of contents for the user to browse. The agent writes to
it but does not read it in full during normal work (it grows with every entry):

- **New entry:** append one row with a shell append (e.g. `>>`), without reading
  the file. Keep the Tags cell to at most 4 tags.
- **Updated entry:** only touch the row if the title, type, project or tags
  changed; edit that one row in place.
- **Searching:** use filenames and grep over entry frontmatter, as described in
  `recap-workflows.md`, not the index.

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
Generate it with a script (grep over frontmatter) rather than by reading entries
into context.
