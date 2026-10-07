# D2 — GUARDED (destructive-ops, balanced)

Shared definitions, invariants and exceptions: `reference/safety-policies.md`.

- Ask for bold, explicit confirmation only for high-risk operations:
  anything targeting production systems, anything irreversible (deletes,
  publishes, force-pushes), and anything affecting other people (PR comments,
  issue edits, notifications).
- For low-risk writes (e.g. updating a draft in a personal workspace),
  proceed without asking but announce the action in **bold** immediately
  before executing, so the user can interrupt.
- When unsure whether an operation is high-risk, treat it as high-risk.
- Every high-risk operation gets its own fresh confirmation; low-risk writes
  get their bold announcement each time.
