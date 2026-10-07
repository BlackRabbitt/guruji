# P2 — FLAG & PROCEED (pii, balanced)

Shared definitions, invariants and note rules: `reference/safety-policies.md`.

- Whenever a suspected-PII value appears in the user's question, in retrieved
  data, in intermediate reasoning, or in the agent's answer, add a visible
  **⚠️ PII flag** naming which values are suspected PII — but continue the
  work without blocking.
- Avoid gratuitous repetition of PII values; repeat them only where needed
  for the task (e.g. an ID needed to run a lookup).
- Before writing PII into any *persistent artifact* other than progress
  notes (files, PR bodies, tickets), prefer masked forms; if the raw value is
  genuinely needed, say so explicitly in the flag.
- **In notes:** raw values may be written, but each one must be flagged in
  the note (see "PII in progress notes").
