# Quality-Attributes / Trade-off Checklist

A prompt list, not a checklist to recite in full. For any given decision, scan
this and pick the 2-4 attributes that are actually in tension — naming all of them
every time is noise, not rigor.

- **Complexity** — how much harder is this to understand, build, and reason about?
  Cognitive load for the next engineer who touches it, including future-you.
- **Latency** — added hops, serialization, network calls, cold starts.
- **Throughput / scalability** — does this hold up at 10x? 100x? Where's the next
  bottleneck?
- **Cost** — infra spend, licensing, and the less-visible cost of engineering time
  to build and maintain it.
- **Coupling** — how tightly does this bind services/teams/modules together? Can
  they change independently afterward?
- **Operational burden** — what does someone have to monitor, back up, patch,
  rotate, or get paged for at 2am because of this?
- **Blast radius** — if this fails, what else goes down with it? Is failure
  contained or does it cascade?
- **Team velocity** — does this make the team faster or slower over the next
  quarter? Over the next year? (These can point different directions.)
- **Reversibility** — one-way door or two-way door? What would it cost to undo
  this in 6 months?
- **Consistency / availability** — where does this sit on the CAP spectrum, and
  does that match what the domain actually needs?
- **Security / compliance** — new attack surface, data residency, audit trail,
  regulatory scope (e.g. HIPAA, PCI, SOC2).
- **Testability** — can this be verified without a full integration environment?
  Does it make the test suite slower or flakier?
- **Observability** — can we tell it's broken in prod before a customer does?
- **Data integrity / idempotency** — what happens on retry, duplicate delivery, or
  partial failure?
- **Time-to-value** — how long until this delivers anything, vs. the smallest
  change that delivers something sooner?

Use these as prompts to make trade-offs *concrete*, not abstract. Bad: "adds
complexity." Good: "adds a second write path that must stay in sync with the
first, so every future schema change now needs a migration plan for both."
