# Safety Policies — Destructive Operations & PII

This file defines the selectable safety policies that govern how the agent
handles (a) destructive operations and (b) personally identifiable information
(PII) while working with the user. It is generic and shareable; the user's
actual selection lives in `data/profile.md` under "Safety Policy Selection".

This file holds what applies at every level. Each level's own rules live in
`reference/safety/`; load only the files for the active levels.

| Domain | Level | One-line summary | File |
|---|---|---|---|
| destructive-ops | D1 HARD GATE (strictest) | bold explicit confirmation before every destructive op | `safety/d1-hard-gate.md` |
| destructive-ops | D2 GUARDED | confirm high-risk ops, announce low-risk writes in bold | `safety/d2-guarded.md` |
| pii | P1 REDACT & GATE (strictest) | mask suspected PII, reveal only on explicit request | `safety/p1-redact-gate.md` |
| pii | P2 FLAG & PROCEED | visible ⚠️ PII flag naming suspected values, work continues | `safety/p2-flag-proceed.md` |

## How selection works

- There are two independent policy domains: **destructive-ops** and **pii**.
  Each domain has two strictness levels. The user picks one level per domain.
- If `data/profile.md` contains no "Safety Policy Selection" section, ask the
  user to choose — once, briefly, using the one-line summaries above — then
  record the selection in the profile. Until a selection exists, apply the
  **strictest** level of both domains (D1 + P1) by default.
- The user can switch levels at any time by saying so (e.g. "switch my PII
  policy to flag-and-proceed"); update the profile when they do.
- These policies are behavioral guardrails like the rigor rule: they apply in
  every session and are never silently relaxed, regardless of urgency.

## Domain 1 — Destructive operations (MCP tools, APIs, terminal CLIs)

"Destructive" = anything that is not purely read-only against state outside
the local working tree: create/update/delete via any MCP server, direct API
calls that write (e.g. `curl -X POST/PUT/DELETE`), and terminal commands that
mutate remote or shared state (`git push`, `gh pr edit`, `gh pr comment`,
re-running CI/GitHub Actions, package publishes, cloud CLI mutations,
deleting resources outside the immediate task's scratch space).

### Shared invariants (both levels)

- Read-only operations (search/get/list/queries, local builds/tests) are
  never gated.
- When in doubt whether something is a write — ask.
- Confirmation prompts are **bold and visually prominent**, e.g.
  "**⚠️ CONFIRM: this will UPDATE/DELETE <target> on <system> — proceed?**",
  and name the exact tool/command, the operation, and the target.
- **Confirmations are single-use and never carried forward.** A confirmation
  covers exactly one execution of exactly the operation it named. A previous
  "yes" — even for an identical operation earlier in the same session, or an
  instruction like "...and then push it" from an earlier request — never
  authorizes a later operation. "The user asked me to do this last time" is
  never grounds for skipping a gate this time.

### User-defined exceptions

The profile may list exceptions to the active D level under
"Safety Policy Selection" → `exceptions`. An excepted operation runs without
a confirmation prompt. Each exception must name:

- **tool/system**: the MCP server, CLI, or repo it applies to;
- **operations**: which writes/deletes are covered;
- **scope**: which targets (e.g. a specific repo/branch, all resources of
  that tool, or only resources created in the current session);
- **notice**: `silent`, or the bold info line to post after each operation.

Rules:
- Exceptions are read narrowly. Anything outside the named tool, operations,
  or scope falls back to the active D level.
- "Created in the current session" means the agent created the resource
  earlier in this same session and can point to that creation. A resource
  created in a previous session, or by anyone else, is out of that scope.
- If there is any doubt about whether an operation falls inside an
  exception's scope, treat it as gated.
- Exceptions never cover secrets, and never relax the PII policy. PII flags
  still apply to content written under an exception.

## Domain 2 — PII (personally identifiable information)

"Suspected PII" includes, non-exhaustively: names of real people (customers,
employees), email addresses, phone numbers, physical addresses, birthdates,
government/insurance/policy/contract numbers, bank details, health-related
data, and pseudonymous identifiers that resolve to a person (customer UUIDs,
integration IDs, profile IDs, session IDs tied to individuals, referral codes
bound to a person). Under GDPR, pseudonymous identifiers are still personal
data. When in doubt, treat a value as PII.

### Shared invariants (both levels)

- The flag obligation covers ALL channels: the user's questions, tool
  results, the agent's reasoning, and final answers.
- PII flags are informational, never accusatory — the goal is awareness of
  what is flowing through the conversation and into third-party AI systems.

## PII in progress notes

How PII is written into `data/` progress notes (and the profile) is not a
separate setting: it **always follows the active P level**. Each P level file
has an **"In notes"** rule, and that rule is the only thing the notes handler
applies.

- **Adding a P level:** every new P level file MUST include an "In notes"
  rule. Notes pick it up automatically; don't duplicate PII rules in
  `SKILL.md`, the schema or the templates, but point here.
- **Missing rule:** if the active P level has no "In notes" rule, or no P
  level is selected, use the strictest level's note behavior (P1, masked) and
  ask the user to define one.
- **Masked (in notes)** means a description or placeholder: "one of 39
  affected leads", "a pet-health customer", `<customer-A>`. Partially
  redacted values (`pet-e770…1a1a`, `j***@gmail.com`) are not masked enough
  for notes, because anyone with database or log access can resolve them.
- **Flagged (in notes)** means the raw value is written with an inline ⚠️
  marker (e.g. `Ajay ⚠️`), and the entry's frontmatter has a `pii:` field
  listing each flagged value with its kind (e.g. `pii: [name: Ajay]`). This
  keeps PII findable for later scrubbing (`grep -rl "^pii:" notes/`).

### Secrets: never written, at any P level

Secrets are not PII, and no P level covers them: passwords, API keys, tokens,
credentials, private keys, bank account and card details, and access-related
details (how to get into systems, access levels, security setup). They are
never written into `data/` notes or the profile. Describe them instead ("the
leaked staging token"), or point to where they're managed ("rotated in AWS
Secrets Manager").
