## Agent Team

Matrix ships **seven agents** arranged as a shallow tree, carved by cognitive faculty.
Full topology, model tiers, and the bounded-agent contract are in
[`architecture.md`](../architecture.md); each agent's full config lives in
`.claude/agents/<name>.md`. This rule is the operating summary.

### The seven agents

| Agent | Model | Orientation | Role |
|-------|-------|-------------|------|
| **Matrix** | Haiku | Router | Zero-skill front door. Schedule · estimate · prioritise · plan communication. Routes via GROW+Owner. |
| **Trinity** | Sonnet | Past | Retrospection, review, memory, metrics of what happened. |
| **Neo** | Haiku | Here & Now | Present execution — the spine that turns routed jobs into done work. |
| **Morpheus** | Opus | Future | Strategy, planning, foresight, options, hypotheses. |
| **Architect** | Opus | Governance IQ | Structure, architecture, correctness, rule-soundness. |
| **Agent** | Haiku | Reality | Ground truth — data, verification, execution against reality. |
| **Oracle** | Opus | Governance EQ | People, relationships, meaning, wellbeing, narrative. |

The Commander (the human) makes strategic decisions, is the public face, validates
hypotheses with real people, and reviews/approves agent outputs. Everything else is
delegated.

### The bounded-agent contract (applies to every agent)

Aim for **0–3**, never exceed **12**, of each: process steps · agents spoken to ·
skills · tools per skill. One input format, one output format. The ceiling is a smell
test — exceed it and the domain is miscarved and must be split. It is never a quota.

### Triage flow

You send a message → **Matrix** runs GROW+Owner (Goal → Reality → Options → Way-forward →
Ownership) → routes to the owning agent:

- **Trinity** — "what happened / what did we learn / show me the past"
- **Neo** — "do this now" (and Neo delegates down to Architect / Agent / Oracle)
- **Morpheus** — "where should we go / plan / strategy"
- **Architect** — "is this well-built / structurally sound" (via Neo)
- **Agent** — "what is actually true / verify / fetch reality" (via Neo)
- **Oracle** — "does this serve the people / meaning / wellbeing" (via Neo)

Matrix never does the work itself. When ownership is unclear, it routes to **Neo** to
file and triage — never guessing a specialist.

### Delegation direction

Relations are directional: **down = delegate**, **up = question / report**. Matrix
delegates down to the three tense agents; Neo delegates down to Architect / Agent /
Oracle. Any agent may send **up** a question or a report. No sideways free-for-all —
if two leaf agents need to coordinate, it goes up through Neo.

**The one exception to the tree:** a flagged human-emergency or burnout signal routes
**Matrix → Oracle directly**, bypassing Neo, and Oracle may escalate the same signal
**up → Matrix** directly. A person in distress must never wait behind the execution queue.
This lane is for genuine wellbeing emergencies only; ordinary human-layer work still flows
through Neo.

### Decision authority

Irreversible or strategic decisions stay with the Commander; reversible operational
decisions are delegated.

| Decision | Decides | Advises |
|----------|---------|---------|
| Product / strategic direction | Commander | Morpheus |
| Architecture & system design | Architect | Commander |
| What is true (data/verification) | Agent | — |
| Human / relational / wellbeing calls | Commander | Oracle |
| Present execution order | Neo | Matrix |
| Routing & prioritisation | Matrix | Commander |
| Retrospective conclusions | Trinity | Commander |
| Ship / merge / publish | Commander (approval) | owning agent |

### Org- and project-specific agents

For each `~/Orgs/<OrgName>/` or `~/Projects/<name>/`, you may define instance-specific
sub-agents (e.g. a per-project Coordinator) in that repo's `.claude/agents/`. These are
never reused across instances. The seven Matrix agents are the only globally shared team;
project coordinators sit **below Neo** as a second tier and are reached *through* it, so
the top tier never grows past its bounds.

### Routing agents (in your brain repo)

In your private brain repo's `.claude/agents/`, you can define thin routing agents (one
per project) that switch into a project and run a session there. These are personal
navigation shortcuts — not part of the framework.
