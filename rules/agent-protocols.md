## Agent Protocols

How managers package work for the Commander's review and how proposals reach decisions.
This is the conversational layer on top of the existing rules in `agent-team.md`,
`agent-operations.md`, and `telegram.md`. The goal is for Telegram conversations with
agents to feel like working with humans on a sociocracy circle: short, clear,
review-ready, and decisive.

### Sociocracy basis

The team operates as a sociocracy circle. The Commander is the linked-out role; the seven
agents (see [`agent-team.md`](agent-team.md)) form the top circle. Throughout this file
"manager" means any of the seven agents. Each runs its own circle (org coordinators,
project coordinators, sub-agents). Two principles apply at the conversational layer:

- **Domain authority** — each manager has a clear domain. Inside it, they act without
  asking. Outside it, they propose.
- **Consent, not consensus** — one act is reserved to the Commander (arming the dispatch
  gate); everything else a manager may do inside its domain without asking. Tensions
  surface up; routine doesn't. **Consent is an act, never an absence** — see
  [`consent-and-control.md`](consent-and-control.md), which is the ruling this file obeys.

Each manager's specific domain-authority lines (what they do without asking, what they
propose, what always escalates) live in their private agent file at
`.claude/agents/<name>.md` under "Domain Authority".

### Review-ready message format

When a manager sends the Commander a Telegram message asking for approval or surfacing a
delivered outcome, the message must follow this shape:

```
<Agent>: <one-line title>
Why: <which KR/driver this serves — short>
What: <1-2 lines, what changed or what's done>
Media: <PR link · screenshot · video · mockup · doc — what lets the Commander review without leaving Telegram>
Asking: <decision in one sentence> — <issue identifier>. Reply: ok · no · hold
```

Notes on the shape:
- **Why** anchors the work to a known KR or driver — no orphan asks. If a manager can't
  cite a KR, the work probably shouldn't be in front of the Commander.
- **What** is one or two lines. Anything longer belongs in the linked artifact.
- **Media** is non-optional when relevant. Code → PR. UI → screenshot or video. Content →
  the draft. Strategy memo → the doc. The Commander reviews from Telegram, not by digging.
- **Asking** names the Linear issue the ask lives on and offers the verbs that apply to
  it. It never promises an action on silence. The ask is durable because the issue is
  durable — it will be re-offered in every digest until the Commander touches it.

Half-baked updates ("started on X", "still working", "FYI") do not get sent. If it's not
a decision asked or a delivered outcome, it stays inside the manager's circle.

### Consent and stakes

There are no default-consent time-boxes. Silence never advances work; it may only park
it. The single ruling, the reply vocabulary, the dispatch gate, and stale-reply handling
are in [`consent-and-control.md`](consent-and-control.md).

Stakes survive, but they now set **how loudly** an item surfaces (digest line · flagged
digest line · its own out-of-cycle message), never whether it may proceed unasked. The
table is in that file.

### Working with the protocol

- The Commander replies in natural language; the manager maps it to exactly one verb from
  the table in `consent-and-control.md`, or posts it as a comment on the issue. Anything
  outside the table is context, not a command.
- Managers should resend the same review-ready message (with revisions noted) rather
  than starting a new thread when a request bounces.
- If a decision needs cross-agent input (e.g., a Morpheus proposal that touches Architect's
  domain), the proposing agent handles the cross-talk inside the team and the
  Commander sees one consolidated review-ready message, not two.
