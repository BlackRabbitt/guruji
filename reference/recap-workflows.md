# Recap Workflows

Both workflows read from `data/notes/` entries (see `notes-profile-schema.md` for
format). Always cite source files so the user can verify/expand. Never fabricate
or embellish beyond what an entry actually says.

## Performance Review Recap

Input: a time range (e.g. "last 6 months", "H1 2026"), and optionally a
project/theme scope.

Steps:
1. Scan `data/notes/index.md` (rebuild first from entry files if it looks stale)
   for entries in range, filtered by project/theme if given.
2. Read the matching entry files in full.
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
1. Map the question type to likely entry `type`/`tags`:
   - Leadership → decisions, wins with cross-team tags
   - Conflict → disagreement/high-pressure entries (often tagged accordingly)
   - Failure/mistake → `type: mistake`
   - Proudest achievement → `type: win`
   - Technical depth → `type: decision` or `type: learning` with technical tags
   - Growth from feedback → entries with a strong "What I'd Do Differently"
     section
2. Search `data/notes/` (via index and/or full-text) for matching entries.
3. Draft a STAR-format answer (Situation / Task / Action / Result) grounded
   *only* in what the entry actually says, citing the source file.
4. If the entry's "Why This Matters" section directly answers what the interview
   question is probing for, lean on it — that's exactly what it's there for.
5. If nothing matches well, say so plainly and suggest the closest partial match
   (if any) rather than inventing a clean story.

## General Recall

Input: a keyword or topic ("what did I do about the N+1 query problem last
year?").

Steps:
1. Grep/search entry files and the index for the keyword across title, tags,
   project, and body.
2. Return matching entries with a one-line summary and file path each. If
   multiple entries match, let the user pick which to expand.
