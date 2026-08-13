# Guruji

A software architecture mentor skill for coding agents (built for Zed's agent
skills system, but the instructions are tool-agnostic). Guruji helps you:

1. Actually solve real architecture/design decisions.
2. Build your own critical thinking about *why* one option beats another,
   instead of just handing you an answer.
3. Keep a running, file-based record of your decisions, mistakes, challenges,
   wins, and learnings — so you can recap real work for performance reviews and
   answer "tell me about a time..." interview questions with specifics instead
   of trying to remember them cold.

## Core principle

Rigor never adapts to who's asking — the same full decision framework runs every
time, whether you're a junior engineer or a staff engineer. What adapts is how
much a given concept gets explained inline, based on what you show you already
know in the conversation.

## What's in here

```
guruji/
├── SKILL.md                     # agent-facing instructions
├── reference/                   # frameworks the agent loads on demand
│   ├── decision-framework.md
│   ├── quality-attributes-checklist.md
│   ├── adr-template.md
│   ├── notes-profile-schema.md
│   ├── recap-workflows.md
│   └── backup-restore.md
├── templates/                   # fillable templates
│   ├── entry-template.md
│   └── profile-template.md
└── data/                        # YOUR data — gitignored, private
    ├── README.md
    └── notes/
        └── index.md
```

## Installing

This is a global agent skill. To install it for Zed's agent:

```sh
git clone <this-repo-url> ~/.agents/skills/guruji
```

(Or copy the directory manually to `~/.agents/skills/guruji` if you're not using
git.) Zed's agent will pick it up automatically — no restart required if the
`~/.agents/skills/` directory already existed.

For a different agent/tool, adapt the destination to wherever it looks for
custom instructions/skills, keeping the directory contents as-is.

## Important: `data/` is private

`data/` holds your personal mentoring profile and your real engineering history
— names, employers, incidents, decisions. It is **gitignored** (see
`.gitignore`) so that cloning/publishing this skill never leaks your personal
data. If you fork or re-publish this repo, always double check `git status
--ignored` shows `data/*` (aside from `data/README.md`) as ignored before
pushing anywhere.

If you want your own notes backed up somewhere durable, that's on you to manage
separately (e.g. a private git repo, or the built-in backup workflow described
in `reference/backup-restore.md` which creates local timestamped archives —
those also stay gitignored).
