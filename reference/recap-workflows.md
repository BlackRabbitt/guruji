# Recap Workflows

All workflows read from `data/notes/` entries (see `notes-profile-schema.md` for
format). Always cite source files so the user can verify/expand. Never fabricate
or embellish beyond what an entry actually says.

## Finding entries cheaply

Don't read `index.md` or whole entries to search — narrow down first, then read
only the entries you'll actually use:

- **By date:** filenames start with the date, so a glob selects a range, e.g.
  `notes/2026-07-*.md notes/2026-08-*.md`. Entries also carry `updated:` when
  they were revised later; include an entry if either date falls in range
  (`grep -l "^updated: 2026-08" notes/*.md`).
- **Frontmatter scan:** list candidates by title/type first, e.g.
  `grep -H -E "^(title|type):" <files>`. Add `project`/`tags` only when
  filtering by project or theme.
- **By signal:** `grep -l -E "^signals:.*\b(conflict|leadership)\b" notes/2*.md`.
- **Full text:** `grep -l -i "<keyword>" notes/2*.md`.

## Reading entries cheaply

Read in two passes. First pass: only the Result and Why This Matters sections
of each candidate (about a quarter of an entry), e.g.

`awk 'FNR==1{print "### " FILENAME} /^## (Result|Why This Matters)/{on=1} /^## /&&!/^## (Result|Why This Matters)/{on=0} on' <files>`

Second pass: read an entry in full only when you need its Situation/Task/Action
(drafting a STAR answer, or the user asks to expand it).

## Performance Review Recap

Input: a time range (e.g. "last 6 months", "H1 2026"), and optionally a
project/theme scope.

Steps:
1. Select entries in range by filename/`updated:` (see above), and scan their
   frontmatter to filter by project/theme if given.
2. Read the matching entries' Result and Why This Matters sections (first pass
   above). Open a full entry only if those sections aren't enough to summarize it.
3. Synthesize a **themed** summary, not a chronological dump. Suggested themes
   (adapt to what's actually present — don't force empty categories):
   - Technical decisions & impact
   - Incidents/bugs handled well (root cause + prevention)
   - Cross-team or leadership moments
   - Notable wins
   - Growth/learning demonstrated (mistakes turned into structural improvements)
4. For each theme, list 2-4 sentence summaries per relevant entry, citing the
   source file path so the user can open it and expand for a performance-review
   doc or self-review.
5. If a time range has very few or no entries, say so plainly rather than
   padding — that's useful signal too (maybe logging was skipped, or it was a
   quiet period).

## Interview Prep (STAR)

Input: a question type — leadership, conflict, failure/mistake, proudest
achievement, technical depth, or growth from feedback (or a specific question the
user pastes in).

Steps:
1. Map the question type to `signals`/`type`:
   - Leadership → `signals: leadership` (also `cross-team`, `mentoring`)
   - Conflict → `signals: conflict`
   - Failure/mistake → `type: mistake`
   - Proudest achievement → `type: win` (prefer ones with `impact`)
   - Technical depth → `type: decision` or `type: learning`, by topic `tags`
   - Growth from feedback → `signals: feedback`, or a strong "What I'd Do
     Differently" section
2. Find candidates by signal/type, falling back to full-text grep, and rank
   them with the first-pass read (see above).
3. Read the best 1-2 candidates in full, then draft a STAR-format answer (Situation / Task / Action / Result) grounded
   *only* in what the entry actually says, citing the source file.
4. If the entry's "Why This Matters" section directly answers what the interview
   question is probing for, lean on it — that's exactly what it's there for.
5. If nothing matches well, say so plainly and suggest the closest partial match
   (if any) rather than inventing a clean story.

## General Recall

Input: a keyword or topic ("what did I do about the N+1 query problem last
year?").

Steps:
1. Full-text grep the entry files for the keyword, then scan the matches'
   frontmatter.
2. Return matching entries with a one-line summary and file path each. If
   multiple entries match, let the user pick which to expand.
