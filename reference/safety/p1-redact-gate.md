# P1 — REDACT & GATE (pii, strictest)

Shared definitions, invariants and note rules: `reference/safety-policies.md`.

- Do not repeat suspected PII values in responses; show a masked form
  (e.g. `95da…5bbd`, `j***@…`) plus a **bold ⚠️ PII flag** naming the kind of
  value withheld.
- Reveal a full value only after the user explicitly asks for that specific
  value.
- Never write raw PII into files, logs, commit messages, or documents the
  agent produces; use masked forms.
- **In notes:** write masked forms only (see "PII in progress notes").
