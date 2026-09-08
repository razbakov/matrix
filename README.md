# Matrix

**A bounded-cognition agent operating system.** The successor to
[ikigai-team](https://github.com/razbakov/ikigai-team).

Matrix is a wiki of governance rules, agent personas, and operating patterns that any
human + AI commander can adopt to run a portfolio of work through a small, bounded team
of AI agents. You import it into your private brain repo and inherit the whole framework.

## Why Matrix (and why it replaces ikigai-team)

ikigai-team carved its agent team by **function** — a manager per topic (ops, code,
content, strategy, community, coaching). That works until the topics multiply and the
managers accrete responsibilities, and you are back to god-agents and an everyone-talks-
to-everyone mesh.

Matrix carves by **cognitive faculty** instead, and enforces a hard **bounded-agent
contract** so cognitive load stays capped:

- Each agent aims for **0–3** (and never more than **12**) of everything it has —
  process steps, agents it talks to, skills, tools per skill.
- The **front door owns zero skills**. It only routes.
- Functional work (content, code, community, coaching) is a **skill dispatched through**
  the cognitive agents — not a permanent seat at the table.
- Every agent must **check the capability workbook before starting any work** and declare
  which skills it used. Bounded cognition without capability retrieval just means agents
  hand-roll work that a skill already does.

The result is seven agents that never grow past their bounds, arranged as a shallow tree.

## The seven agents

| Agent | Model | Orientation | Owns |
|---|---|---|---|
| **Matrix** | Haiku | Router | Schedule · estimate · prioritise · plan communication. Zero skills. |
| **Trinity** | Sonnet | Past | Retrospection, review, memory, metrics of what happened. |
| **Neo** | Haiku | Here & Now | Present execution — the spine that turns routed jobs into done work. |
| **Morpheus** | Opus | Future | Strategy, planning, foresight, options. |
| **Architect** | Opus | Governance IQ | Structure, architecture, correctness, rule-soundness. |
| **Agent** | Haiku | Reality | Ground truth — data, verification, execution against reality. |
| **Oracle** | Opus | Governance EQ | People, relationships, meaning, wellbeing, narrative. |

Named after *The Matrix*. Full spec in **[architecture.md](architecture.md)**.

## Getting started

Add one import line to your private brain repo's `CLAUDE.md`:

```
@/absolute/path/to/matrix/CLAUDE.md
```

Then define your instance-side config (identity, projects, tokens) in that private repo.
The framework ships the *timeless* layer: the seven agents (`.claude/agents/`) and the
operating rules (`rules/`). It ships **no** personal data.

## Layout

```
matrix/
├── CLAUDE.md          # framework root — imports the rules
├── architecture.md    # the canonical agent architecture + bounded contract
├── MIGRATION.md       # ikigai-team → Matrix mapping
├── rules/             # proven operating knowledge (carried from ikigai-team)
└── .claude/agents/    # the seven cognitive agents
```

## Status

v0 scaffold. The operating rules are carried over from ikigai-team and are stable;
`rules/agent-team.md` has been rewritten for the seven-agent roster. Cutover of a live
instance (e.g. `~/Orgs/ikigai`) from ikigai-team to Matrix is a **separate, deliberate
step** — not done yet.
