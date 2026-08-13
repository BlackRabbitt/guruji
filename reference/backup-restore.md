# Backup & Restore Procedure

`data/` holds personal, sensitive, hard-to-reconstruct history. Treat backup and
restore operations carefully — they are the one place in this skill where a
mistake is not easily undone.

## Backup (on request only)

1. Create a timestamped archive of the entire `data/` directory (excluding
   `data/backups/` itself, to avoid nesting backups inside backups), e.g.:
   `data/backups/guruji-backup-YYYYMMDD-HHMMSS.tar.gz` or a plain timestamped
   directory copy — either is fine, prefer whatever's simplest given available
   tools.
2. Never delete or move the original `data/` contents as part of a backup.
3. Confirm to the user where the backup landed and what it contains (counts by
   type, date range covered), so they know it's safe to rely on it.
4. Backups are also personal data — they live under `data/backups/`, which is
   covered by the same `.gitignore` rule as the rest of `data/`.

## Restore (from a given backup path)

1. **Always confirm merge-vs-overwrite before touching anything existing.** Ask
   explicitly: "Should I merge these entries into the current `data/notes/`
   (keeping both sets, erroring/renaming on filename collisions) or overwrite
   `data/` entirely with the backup?" Do not proceed on an assumption.
2. **Validate structure before applying anything**: confirm the backup contains
   the expected shape (entry files with frontmatter matching the schema in
   `notes-profile-schema.md`, optionally a profile.md and index.md). If the
   structure looks off, stop and report what's wrong rather than partially
   applying it.
3. Apply the restore according to the confirmed mode (merge or overwrite).
   - Merge: copy in entries that don't already exist by filename; if a filename
     collision has different content, flag it to the user rather than silently
     picking one side.
   - Overwrite: replace `data/` contents with the backup's contents.
4. **Rebuild `data/notes/index.md` from scratch by scanning the actual restored
   entry files' frontmatter** — never trust an index that came bundled with the
   backup, since it may be stale relative to the entries.
5. Report a clear summary when done: total entries by type, date range covered,
   and whether a profile.md was present/restored.
