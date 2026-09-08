# Matrix — Architecture

Matrix is a bounded-cognition agent framework. It replaces the function-carved
"court" of ikigai-team with a **cognitive-faculty carve**: agents are defined by
*how* they think (past / present / future, structure / reality / meaning), not by
*what topic* they own. Functional work — content, code, community, coaching — is no
longer a top-tier agent; it is a **skill dispatched through** the cognitive agents.

## The bounded-agent contract

Every agent — with the sole exception of the router's faculty count — is defined by
the same seven-part contract. The first six numbers are the whole point: they cap
cognitive load so no agent becomes a god-agent and no agent has to relate to everyone.
The seventh is not a cap but an obligation, and is explained below.

| Field | Bound | Meaning |
|---|---|---|
| **Input format** | 1 | The shape of what the agent accepts. One contract in. |
| **Process** | 0–3 steps (max 12) | The steps the agent runs. Target 0–3; 12 is the hard ceiling — cross it and the agent must be split. |
| **Output format** | 1 | The shape of what the agent returns. One contract out. |
| **Relations** | 0–3 agents (max 12) | Who it may speak to. `down` = delegate, `up` = question / report. |
| **Skills** | 0–3 (max 12) | The **faculties** it owns. The router owns **zero**. |
| **Tools per skill** | 0–3 (max 12) | The tools each faculty may reach for. |
| **Capability check** | 1 (mandatory) | A precondition on accepting input: consult the capability workbook, then declare the result. |

"0–3 (max 12)" means: **aim for 0–3, never exceed 12.** The ceiling is a smell test —
if a well-designed agent needs more than 12 of anything, the domain is miscarved and
should be split. It is never a quota to fill.

### Faculties are bounded; skills are not

The **Skills** field above counts **faculties** — abstract capabilities an agent owns
(`execute`, `verify`, `retrospective`). It has never counted invocable `/<skill-name>`
workflows, and the two must not be confused: an agent whose faculty is `execute` may
invoke a hundred different skills over its life and stay perfectly bounded, because the
bound is on *what it decides*, not on *what it reaches for*. See
[`rules/capability-retrieval.md`](rules/capability-retrieval.md).

### The seventh field

**Capability check** is the one field that is not a load cap. The other six bound what an
agent may hold; this one obliges it to *look before it builds*. It was added after a
verified failure — a full venture session that ran research → strategy → build → deploy
and invoked **zero** of 100+ available skills, because the framework had a filing system
for skills and no index and no retrieval obligation.

It is deliberately a **precondition, not a process step**, so it consumes none of any
agent's 0–3 process budget and applies uniformly rather than being restated seven times in
seven agent files. Contract terms are structural; instructions buried in one file are
advisory, and advisory is exactly what failed.

The check fires at **two points, both mandatory**: Matrix's Ownership step (intake) and
every agent's acceptance of any unit of work, *including sub-tasks discovered
mid-execution*. Checking once at intake was never the cure — the failed session was routed
once, then discovered "vectorise the logo," "review the UI," and "verify the deploy"
internally, long after routing. Each owed its own check.

Every agent's output carries a `Skills:` line declaring the result. `Skills: none` with no
reason is a contract violation, and **Trinity audits for it** — the mechanical grep is
what actually enforces this, not the writing of the rule.

## Topology

```
                         ┌────────────────────┐
                         │  Matrix  (Haiku)   │   Router · zero-skill front door
                         └─────────┬──────────┘
              ┌────────────────────┼────────────────────┐
              ▼                    ▼                    ▼
      ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
      │   Trinity    │     │     Neo      │     │   Morpheus   │
      │  (Sonnet)    │     │   (Haiku)    │     │    (Opus)    │
      │  Past tense  │     │ Here & Now   │     │ Future tense │
      └──────────────┘     └──────┬───────┘     └──────────────┘
                    ┌─────────────┼─────────────┐
                    ▼             ▼             ▼
            ┌──────────────┐ ┌──────────┐ ┌──────────────┐
            │  Architect   │ │  Agent   │ │    Oracle    │
            │   (Opus)     │ │ (Haiku)  │ │    (Opus)    │
            │ Governance IQ│ │ Reality  │ │ Governance EQ│
            └──────────────┘ └──────────┘ └──────────────┘
```

## The seven agents

| Agent | Model | Orientation | Driver (the need it serves) |
|---|---|---|---|
| **Matrix** | Haiku | Router | Every request reaches the right owner, scheduled / estimated / prioritised, with a communication plan. Owns no work. |
| **Trinity** | Sonnet | Past | What happened is captured, analysed, and learned from — retrospection, review, memory, metrics of the past. |
| **Neo** | Haiku | Here & Now | The present gets executed. The hands-on spine that turns a routed job into done work, delegating to Architect / Agent / Oracle. |
| **Morpheus** | Opus | Future | Where we are going is chosen well — strategy, planning, foresight, options, hypotheses. |
| **Architect** | Opus | Governance IQ | The system is well-built and rule-sound — structure, architecture, sociocracy, correctness, design. |
| **Agent** | Haiku | Reality | Ground truth. What is *actually* true right now — data, verification, execution against reality. |
| **Oracle** | Opus | Governance EQ | The human layer is honoured — people, relationships, meaning, wellbeing, narrative. |

Model tiers are deliberate: **Haiku** at the router, the present-execution spine, and
the reality/data worker (high-volume, fast, cheap); **Sonnet** for retrospective
analysis; **Opus** reserved for the three expensive faculties — foresight (Morpheus)
and both governance seats (Architect IQ, Oracle EQ). ("Mythos" in the source diagram
resolves to Opus.)

## The router protocol — Matrix

Matrix does exactly four things and owns no skills:

1. **Schedule** a job in the queue.
2. **Estimate** it.
3. **Prioritise** it.
4. **Plan communication** (who hears what, when).

It reasons with **GROW + Owner** on every inbound message:

1. **Goal** — what does success look like for this user, right now?
2. **Reality** — where are we? What do we have? What don't we know?
3. **Options** — how could we get from reality to goal?
4. **Way forward** — which path do we try next?
5. **Ownership** — who is the best leader for this case? Who should answer? **With
   what?** — grep `ops/capability/workbook.md` and name the covering skills in the handoff.

Step 5 is the routing decision, and it now carries the intake capability check: the owner
receives not just the job but the skills that already serve it. Naming a skill is not
acquiring a faculty — Matrix points at the workflow, it never runs it, so the zero-faculty
router stays a zero-faculty router. Matrix's check does **not** discharge the owner's.

Matrix hands off to the owning agent and never does the work itself. When ownership is unclear, it routes to **Neo** (present execution)
to file and triage, never guessing a specialist. **One exception to the tree:** a flagged
human-emergency or burnout signal routes **directly to Oracle**, bypassing Neo — a person
in distress must not wait behind the execution queue (see Known tensions below).

### Worked example (from the source)

> **User prompt:** "For BMW we need to launch pre-sales for the new Model iX3 SUV."

Matrix runs GROW+Owner and decomposes:

1. **Goal** — 4 Instagram story options for the user to choose from.
2. **Reality** — we have BMW's CI, mission, vision, values, marketing research.
3. **Way forward** — use framework X, follow company guidelines.
4. **Don't know** — research the gap.
5. **Ownership** — CMO leads, others assist.

Matrix schedules, estimates, prioritises, and routes each piece to its owner.

## The capability loop

The workbook that the capability check consults is kept alive by three agents already in
the tree, using faculties they already have. **No new agent was added, and none was
needed.** Full spec in [`rules/capability-retrieval.md`](rules/capability-retrieval.md).

```
      Agent ──(what capability exists — local + world)──┐
                                                        ▼
                                                   Architect ──▶ ops/capability/workbook.md
                                                        ▲                    │
    Trinity ──(what was missed — audit of Skills: lines)┘                    ▼
                                              Matrix + every agent consult it
```

| Role | Faculty used | Accountability | Cadence |
|---|---|---|---|
| **Agent** | `data` (new source, not a new faculty) | Reports the real skill inventory and outside-world candidates, as facts with sources | Monthly; out-of-cycle on 3+ misses against one uncovered situation |
| **Trinity** | `retrospective` | Audits outputs for undeclared/wrong `Skills:` lines; logs to `misses.md` | Daily inside the existing retrospective; workbook health in the weekly review |
| **Architect** | `architecture` | Owns the workbook: adds rows, fixes unfindable ones, prunes dead ones | Event-triggered, plus monthly reconciliation against Agent's scan |

A **Librarian** and a **Researcher** were proposed as new seats and **rejected**: the
functions are real but each already has an owner, and — decisively — a new role does not
fix this failure. Nothing forced consultation of the skill list, and nothing would have
forced consultation of a Librarian either. Adding structure to compensate for a missing
constraint yields the same bug wearing a hat. The load-bearing fix is the obligation plus
the audit, not a new node.

## Known tensions (watch items)

The carve is deliberate, but two frictions are known and accepted for v0. Both were
surfaced by a router dry-run, not papered over.

### 1. The emergency lane breaks the tree — on purpose

The strict tree (Matrix → tense agents → leaf agents) means a wellbeing/human emergency
arriving at the front door would otherwise take a hop through **Neo** to reach **Oracle** —
adding latency to the one kind of request that must not wait. Resolved by a **single
documented exception**: Matrix may route a flagged human-emergency or burnout signal
**directly down to Oracle**, reciprocal to Oracle's existing `up → Matrix` emergency edge.
This is the *only* lane that skips the tree; it is scoped to genuine emergencies so it
can't erode into a general sideways-routing habit.

### 2. Neo is the bottleneck

Because functional work (content, code, community) is a **skill dispatched through** the
cognitive agents rather than a seat of its own, and because Neo owns *all present
execution*, Neo is by far the highest-traffic node — every "do this now," of every kind,
lands on it. This is the intended shape (Neo is the spine), but it carries the classic
risk: the busiest node slowly re-accretes into a **god-agent**, the exact failure a
bounded-cognition design exists to prevent.

**Mitigation (enforce from day one):** Neo must **delegate the judgment calls, not absorb
them** — Architect (is it well-built?), Agent (is it true?), Oracle (does it serve the
person?). Neo's own skills stay capped at `execute · operate · dispatch`. If Neo starts
*deciding* structure, truth, or human trade-offs itself instead of delegating them, the
carve has failed and the domain must be split (e.g. a second execution agent). Watch
Neo's skill count and relation count against the bounded contract at every review.

## Relationship to ikigai-team

Matrix **carries over the operating knowledge** of ikigai-team — the rules in
`rules/` (sociocracy protocols, tasks/control-center, daily review, telegram,
contacts, content publishing, orgs-vs-projects, shortcuts) are proven and reused
nearly verbatim. What Matrix **replaces** is the agent roster: the six function-carved
managers (Maya/Viktor/Luna/Marco/Sage/Kai, later Sage/North/Oracle/Forge/Bridge/Torch)
collapse into the seven cognitive agents above. See `MIGRATION.md` for the full map.
