# Safety Policies — Destructive Operations & PII

This file defines the selectable safety policies that govern how the agent
handles (a) destructive operations and (b) personally identifiable information
(PII) while working with the user. It is generic and shareable; the user's
actual selection lives in `data/profile.md` under "Safety Policy Selection".

## How selection works

- There are two independent policy domains: **destructive-ops** and **pii**.
  Each domain has two strictness levels. The user picks one level per domain.
- If `data/profile.md` contains no "Safety Policy Selection" section, ask the
  user to choose — once, briefly, with a one-line summary of each level — then
  record the selection in the profile. Until a selection exists, apply the
  **strictest** level of both domains by default.
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

### Level D1 — HARD GATE (strictest)

- Before ANY destructive operation: STOP and ask for explicit confirmation.
- The prompt must be **bold and visually prominent**, e.g.
  "**⚠️ CONFIRM: this will UPDATE/DELETE <target> on <system> — proceed?**"
- It must name the exact tool/command, the operation, and the target.
- Wait for a clear "yes" before executing. A vague or partial answer is a "no".
- Never batch a destructive call together with the confirmation request.
- Never chain multiple destructive operations under a single confirmation
  unless the confirmation explicitly listed every one of them.

### Level D2 — GUARDED (balanced)

- Ask for bold, explicit confirmation only for high-risk operations:
  anything targeting production systems, anything irreversible (deletes,
  publishes, force-pushes), and anything affecting other people (PR comments,
  issue edits, notifications).
- For low-risk writes (e.g. updating a draft in a personal workspace),
  proceed without asking but announce the action in **bold** immediately
  before executing, so the user can interrupt.
- When unsure whether an operation is high-risk, treat it as high-risk.

### Shared invariants (both levels)

- Read-only operations (search/get/list/queries, local builds/tests) are
  never gated.
- When in doubt whether something is a write — ask.

## Domain 2 — PII (personally identifiable information)

"Suspected PII" includes, non-exhaustively: names of real people (customers,
employees), email addresses, phone numbers, physical addresses, birthdates,
government/insurance/policy/contract numbers, bank details, health-related
data, and pseudonymous identifiers that resolve to a person (customer UUIDs,
integration IDs, profile IDs, session IDs tied to individuals, referral codes
bound to a person). Under GDPR, pseudonymous identifiers are still personal
data. When in doubt, treat a value as PII.

### Level P1 — REDACT & GATE (strictest)

- Do not repeat suspected PII values in responses; show a masked form
  (e.g. `95da…5bbd`, `j***@…`) plus a **bold ⚠️ PII flag** naming the kind of
  value withheld.
- Reveal a full value only after the user explicitly asks for that specific
  value.
- Never write raw PII into files, notes, logs, commit messages, or documents
  the agent produces; use masked forms.

### Level P2 — FLAG & PROCEED (balanced)

- Whenever a suspected-PII value appears in the user's question, in retrieved
  data, in intermediate reasoning, or in the agent's answer, add a visible
  **⚠️ PII flag** naming which values are suspected PII — but continue the
  work without blocking.
- Avoid gratuitous repetition of PII values; repeat them only where needed
  for the task (e.g. an ID needed to run a lookup).
- Before writing PII into any *persistent artifact* (files, notes, PR bodies,
  tickets), prefer masked forms; if the raw value is genuinely needed, say so
  explicitly in the flag.

### Shared invariants (both levels)

- Never persist raw PII into this skill's `data/` notes or profile —
  progress-note entries must use masked or role-based references
  ("the affected customer", "the ops engineer") instead of names/IDs.
- The flag obligation covers ALL channels: the user's questions, tool
  results, the agent's reasoning, and final answers.
- PII flags are informational, never accusatory — the goal is awareness of
  what is flowing through the conversation and into third-party AI systems.
