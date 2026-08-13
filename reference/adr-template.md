# ADR Template

Use this when drafting a lightweight Architecture Decision Record with the user.
Fill in every section — if something is genuinely not applicable, say "N/A" and
why, rather than omitting it. Save the finished ADR to wherever the user's project
keeps ADRs (ask if unclear, e.g. `docs/adr/NNNN-title.md`); this is a project
artifact, not personal data, so it does not belong under this skill's `data/`.

```markdown
# ADR NNNN: <short, decision-oriented title>

- Status: Proposed | Accepted | Superseded by ADR-XXXX | Deprecated
- Date: YYYY-MM-DD
- Deciders: <names/roles>
- Related: <links to tickets, RFCs, prior ADRs>

## Context

What problem are we solving, for whom, under what constraints (scale, team,
latency budget, cost ceiling, compliance, deadline)? Is this a one-way door?

## Options Considered

### Option A: <name, include "smallest possible change" if it's an option>
Brief description.

### Option B: <name>
Brief description.

### Option C: <name, if applicable>
Brief description.

## Trade-offs

For each option, name the 2-4 quality attributes that actually matter here
(complexity, latency, cost, coupling, operational burden, blast radius, team
velocity, reversibility, etc. — see `quality-attributes-checklist.md`):

| Option | Pros | Cons |
|---|---|---|
| A | | |
| B | | |
| C | | |

## Decision

Which option was chosen, and why — grounded in *this* project/team/moment, not
generic best practice.

## Consequences

- What does this commit us to?
- What future option does this close off or make more expensive?
- What should be monitored or revisited, and what would trigger revisiting it
  (time, scale threshold, incident)?

## Rollback / Reversal Plan

If this turns out to be wrong, what does undoing it look like?
```
