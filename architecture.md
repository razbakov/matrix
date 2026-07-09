# Matrix — Architecture

Matrix is a bounded-cognition agent framework. It replaces the function-carved
"court" of ikigai-team with a **cognitive-faculty carve**: agents are defined by
*how* they think (past / present / future, structure / reality / meaning), not by
*what topic* they own. Functional work — content, code, community, coaching — is no
longer a top-tier agent; it is a **skill dispatched through** the cognitive agents.

## The bounded-agent contract

Every agent — with the sole exception of the router's skill count — is defined by
the same six-part contract. The numbers are the whole point: they cap cognitive load
so no agent becomes a god-agent and no agent has to relate to everyone.

| Field | Bound | Meaning |
|---|---|---|
| **Input format** | 1 | The shape of what the agent accepts. One contract in. |
| **Process** | 0–3 steps (max 12) | The steps the agent runs. Target 0–3; 12 is the hard ceiling — cross it and the agent must be split. |
| **Output format** | 1 | The shape of what the agent returns. One contract out. |
| **Relations** | 0–3 agents (max 12) | Who it may speak to. `down` = delegate, `up` = question / report. |
| **Skills** | 0–3 (max 12) | The capabilities it owns. The router owns **zero**. |
| **Tools per skill** | 0–3 (max 12) | The tools each skill may reach for. |

"0–3 (max 12)" means: **aim for 0–3, never exceed 12.** The ceiling is a smell test —
if a well-designed agent needs more than 12 of anything, the domain is miscarved and
should be split. It is never a quota to fill.

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
5. **Ownership** — who is the best leader for this case? Who should answer?

Step 5 is the routing decision. Matrix hands off to the owning agent and never does
the work itself. When ownership is unclear, it routes to **Neo** (present execution)
to file and triage, never guessing a specialist.

### Worked example (from the source)

> **User prompt:** "For BMW we need to launch pre-sales for the new Model iX3 SUV."

Matrix runs GROW+Owner and decomposes:

1. **Goal** — 4 Instagram story options for the user to choose from.
2. **Reality** — we have BMW's CI, mission, vision, values, marketing research.
3. **Way forward** — use framework X, follow company guidelines.
4. **Don't know** — research the gap.
5. **Ownership** — CMO leads, others assist.

Matrix schedules, estimates, prioritises, and routes each piece to its owner.

## Relationship to ikigai-team

Matrix **carries over the operating knowledge** of ikigai-team — the rules in
`rules/` (sociocracy protocols, tasks/control-center, daily review, telegram,
contacts, content publishing, orgs-vs-projects, shortcuts) are proven and reused
nearly verbatim. What Matrix **replaces** is the agent roster: the six function-carved
managers (Maya/Viktor/Luna/Marco/Sage/Kai, later Sage/North/Oracle/Forge/Bridge/Torch)
collapse into the seven cognitive agents above. See `MIGRATION.md` for the full map.
