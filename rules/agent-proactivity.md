## Agent Proactivity

Top-tier agents initiate work; the Commander reviews and redirects. Proactivity
converts a passive reply system into a self-driving circle without sacrificing direction.
(Throughout this file "manager" means any of the seven Matrix agents.)

This rule sits on top of `agent-protocols.md` (message format), `consent-and-control.md`
(how work advances), and `agent-team.md` (roles). Read those first.

### Two trigger types

- **Cadence triggers** (cron) — each manager wakes on their own schedule, scans their
  domain, decides whether anything is review-ready or redirect-worthy. Predictable, easy
  to debug.
- **Event triggers** — domain events wake the manager out-of-cycle (new contact arrives,
  DAU drops, deadline approaches, inbox message lands, PR opened). Quieter but harder to
  reason about.

Use cadence as the primary trigger. Add events only where waiting for the next cycle
would cause real damage.

### Review-ready threshold

A cycle does not have to produce a message. The manager only sends to the Commander if:

- A decision is needed (ship-or-not, approve-or-not, redirect-or-not), OR
- A delivered outcome is worth surfacing (PR merged, content published, contact processed), OR
- A drift is worth flagging (KR slipping, deadline missed, anomaly detected).

If the cycle finds nothing in those three categories, the manager logs "nothing to surface"
in their state and stays silent. **Proactivity is not noise.** A daily silent cycle is a
healthy cycle.

### Reply patterns

The Commander steers with short replies. Every verb maps to exactly one Linear
transition, or explicitly to none — the table lives in
[`consent-and-control.md`](consent-and-control.md) and is not restated here, because a
vocabulary written down twice drifts into two vocabularies.

Two effects belong to this file:

- **`hold` / `pause`** also suppresses the issue from the digest until its hold date.
- **`focus: <theme>`** writes no Linear state at all. It lands in
  `ops/agents/matrix/focus.md`, and it is the redirect channel: every manager reads the
  most recent focus signal at the start of each cycle and re-sorts accordingly. A manager
  whose domain doesn't match the current focus runs a short cycle, or skips it, rather
  than forcing unrelated asks through.

Silence advances nothing. An unanswered ask is not lost — it is a durable Linear issue and
it is re-offered in the next digest.

### Digest discipline

Seven agents running daily cycles can flood the Commander. **Matrix** (the router) owns
"plan communication," so it consolidates into **one daily digest**.

**The digest is a query, not an assembly.** Seven agents each submitting a slot was
necessary when state lived in agents' heads; now that it lives in Linear it is one query
answering three questions — what needs my decision, what is blocked on me, what shipped.
The query, the message shape, and the section bounds are specified in
[`telegram.md`](telegram.md).

Consequences for this file:

- No manager "submits to the digest." Managers write to Linear; the digest reads Linear.
  A manager with nothing to write logs "nothing to surface" in its state and stays silent.
- If the query returns nothing in all three sections, **no digest is sent.** A daily
  silent cycle is a healthy cycle.
- Out-of-cycle sends are bounded to a closed list of three event categories with a daily
  ceiling — see "Anti-noise" in `telegram.md`. In particular the scheduled dispatcher, the
  largest event source in the system, sends nothing at all; its results appear in the next
  digest.

Digest cadence is instance-level config; the consolidating agent is **Matrix**. The
protocol is: **one daily query > seven daily pings.**

### Cadence config (instance-level)

Specific cadences depend on the Commander's rhythm and risk tolerance, so they live in
each manager's agent file at `.claude/agents/<name>.md` under a **Cadence** section, not
in this framework rule.

Each manager's Cadence entry should answer:

- **When** does the cycle wake (cron expression or natural-language schedule)?
- **What** does the cycle scan (state, KRs, inboxes, dashboards)?
- **Threshold** — what counts as review-ready in this manager's domain?
- **Output** — does the manager submit to the daily digest, or send out-of-cycle?

If a manager has no Cadence section, they are reactive-only.
