# Guruji

Guruji is an agent skill that turns your coding agent into a **software
architecture mentor**. It was built for Zed's agent
skills system, but the instructions are plain markdown and tool-agnostic.

It's not application code — just structured instructions, frameworks, and
templates the agent loads on demand.

## What it does

### 1. Mentors you through real engineering decisions

- Architecture and design choices ("should I use X or Y"), feature planning,
  bug root-causing, and refactor decisions.
- Socratic-first: it restates the decision, asks a few sharp clarifying
  questions, asks for your instinct on genuine learning moments — then reasons
  through a full ADR-lite framework (Context / Options / Trade-offs /
  Recommendation / Consequences) and makes a clear call.
- Offers to draft a lightweight ADR for significant decisions, and closes
  substantive discussions with a short "growth nudge" naming the general
  principle behind the specific call.

**Core principle: rigor never adapts, explanation depth does.** The same full
decision framework runs every time, whether you're a junior or a staff
engineer. What adapts is how much a concept gets explained, based on what you
show you already know in the conversation.

### 2. Keeps a private engineering journal

- Logs your decisions, mistakes, challenges, wins, and learnings as markdown
  files under `data/notes/` — always proposed, never logged silently.
- Recall workflows:
  - **Performance review recaps** — themed summaries of real work over a time
    range, with sources cited.
  - **Interview prep** — STAR-format answers to "tell me about a time..."
    questions, grounded only in what you actually logged.
  - **General recall** — quick keyword search ("what did I do about X?").
- Includes a backup/restore procedure for your notes.

### 3. Enforces your safety policies

Two user-selected policy domains, active in every session:

- **Destructive operations** — `D1 HARD GATE` (explicit confirmation before
  every destructive op) or `D2 GUARDED` (confirm high-risk ops, announce
  low-risk writes).
- **PII** — `P1 REDACT & GATE` (mask suspected PII, reveal only on request) or
  `P2 FLAG & PROCEED` (visible ⚠️ flag, work continues).

Your choice is stored in your profile; until you choose, the strictest levels
apply. Policies never relax under deadline pressure, and raw PII is never
persisted into your notes.

Confirmations are **single-use and never carried forward**: a "yes" covers
exactly one execution of the operation it named. Saying "update it and push"
once doesn't authorize any later push — the agent asks fresh, every time.

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
│   ├── backup-restore.md
│   └── safety-policies.md
├── templates/                   # fillable templates
│   ├── entry-template.md
│   └── profile-template.md
└── data/                        # YOUR data — gitignored, private
    ├── README.md
    ├── profile.md               # created on first setup
    └── notes/
        └── index.md
```

## Installing

This is a global agent skill. To install it for Zed's agent:

```sh
git clone <this-repo-url> ~/.agents/skills/guruji
```

(Or copy the directory manually to `~/.agents/skills/guruji` if you're not
using git.) Zed's agent picks it up automatically.

For a different agent/tool, adapt the destination to wherever it looks for
custom instructions/skills, keeping the directory contents as-is.

## Important: `data/` is private

`data/` holds your personal mentoring profile and your real engineering
history — names, employers, incidents, decisions. It is **gitignored** so that
cloning or publishing this skill never leaks your personal data. If you fork
or re-publish this repo, double check that `git status --ignored` shows
`data/*` (aside from `data/README.md`) as ignored before pushing anywhere.

If you want your notes backed up somewhere durable, manage that separately —
e.g. a private git repo, or the built-in backup workflow in
`reference/backup-restore.md`, which creates local timestamped archives (also
gitignored).
