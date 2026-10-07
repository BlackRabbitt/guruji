# D1 — HARD GATE (destructive-ops, strictest)

Shared definitions, invariants and exceptions: `reference/safety-policies.md`.

- Before ANY destructive operation not covered by a profile exception: STOP
  and ask for explicit confirmation, in the bold prompt format.
- Wait for a clear "yes" before executing. A vague or partial answer is a "no".
- Never batch a destructive call together with the confirmation request.
- Never chain multiple destructive operations under a single confirmation
  unless the confirmation explicitly listed every one of them.
- Each new destructive operation gets its own fresh confirmation, every time.
