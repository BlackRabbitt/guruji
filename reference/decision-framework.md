# Decision Framework (ADR-lite Reasoning)

Use this for any real architectural or design decision — not for trivial calls
(e.g. "should this be a private method"). It's the backbone of every substantive
answer this skill gives, regardless of who's asking.

## What counts as "non-trivial"

A decision is non-trivial if any of these are true:
- It's hard or costly to reverse (new datastore, new service boundary, public API
  shape, schema change, chosen consistency model).
- It affects more than one team, or a contract other people/services depend on.
- It trades off two real quality attributes against each other (e.g. latency vs.
  consistency, velocity vs. correctness).
- Getting it wrong would cause a production incident, a costly migration, or a
  multi-week rework.

If none of these apply, give a direct answer plus a one-line rationale — don't run
the full Socratic loop for e.g. "should I extract this into a helper function."

## The five steps

### 1. Context
What problem, for whom, under what constraints? State it back in your own words
before analyzing, so a wrong assumption surfaces early. Constraints worth
surfacing explicitly:
- Current scale, and a 12-24 month projection (not just "today").
- Consistency/availability requirements.
- Team size and who will own/operate this.
- Latency budget.
- Cost ceiling.
- Compliance/regulatory context.
- Deadline pressure.
- Reversibility — is this a one-way door or can we cheaply undo it later?

Only ask about the constraints that would actually change the recommendation —
don't interrogate for its own sake.

### 2. Options
List at least two genuine alternatives. Always include "smallest possible change"
as a baseline option, even if you don't expect to recommend it — it's the
yardstick everything else is measured against.

### 3. Trade-offs
For each option, name concrete costs, not vague ones. Use
`quality-attributes-checklist.md` as a prompt list, but only surface the 2-4
attributes that actually matter for this case. "This adds complexity" is not a
trade-off; "this adds a second datastore your 3-person team now has to operate,
back up, and reason about during incidents" is.

### 4. Recommendation
Pick one option and justify it based on *this* project/team/moment — not generic
"best practice" or "what FAANG does." Be willing to say "there isn't a clearly
better option, here's how I'd break the tie and why."

### 5. Consequences
- What does this commit us to?
- What future option does it foreclose or make more expensive later?
- What should be monitored or revisited, and on what trigger (time, scale
  threshold, incident)?

## When to offer an ADR

Offer to draft one (`adr-template.md`) when the decision involves any of:
- A new service boundary.
- A new datastore or storage technology.
- A new external dependency (vendor, library with heavy blast radius).
- A breaking API or schema change.
- A major refactor touching multiple teams' contracts.

Don't insist — offer once, proceed with the reasoning either way.
