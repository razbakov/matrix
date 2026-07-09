# Migration — ikigai-team → Matrix

Matrix keeps ikigai-team's **operating knowledge** and replaces its **agent roster**.
Nothing about the daily cadence, sociocracy protocols, tasks/control-center discipline,
telegram surface, or contacts/publishing rules changes in principle — those carry over.
What changes is *who* the agents are and *how the work is carved*.

## The carve changes axis

- **ikigai-team:** carved by **function**. Six managers, each owning a topic
  (ops, engineering, content, strategy, community, coaching), later reframed as a court
  (Sage/North/Oracle/Forge/Bridge/Torch).
- **Matrix:** carved by **cognitive faculty**. Seven agents by *how they think*
  (past / present / future · governance IQ / reality / governance EQ) plus a router.

Functional topics do not disappear — they become **skills dispatched through** the
cognitive agents. "Write the blog post" is a content skill Neo executes; "is the launch
copy true?" is checked by Agent; "does it serve the audience?" is Oracle's call; "is it
strategically right?" is Morpheus's.

## Where each old domain lands

| ikigai-team (court) | Its domain | Lands in Matrix as |
|---|---|---|
| **Router** (Herald/Porter — proposed, never built) | Zero-skill dispatch | **Matrix** — now the named front door, GROW+Owner protocol |
| **Forge** (Police) | Engineering / architecture | **Architect** (design, correctness, structure) + **Agent** (execution against reality) |
| **Forge** (ops half) / dissolved **Maya** / proposed **Steward** | The operational machine — dispatch, daily/weekly cadence, inbox, calendar | **Neo** (present execution) with **Trinity** owning the *review/retrospective* half |
| **North** (General) | Strategy, OKRs, foresight, prioritisation | **Morpheus** (future) — with prioritisation shared to **Matrix** at routing time |
| **Sage** (Queen) | The person, wellbeing, purpose | **Oracle** (Governance EQ) — the human layer |
| **Oracle** (High Priest) | Meaning, symbolic, the wheel | folded into **Oracle** (Governance EQ) — meaning + people are one faculty |
| **Bridge** (Ambulance) | Community, partnerships, contacts | **Oracle** (relationships) governs; **Neo** executes the outreach |
| **Torch** (Firefighters) | Content, growth, SEO | a **skill set** executed by **Neo**, judged by **Oracle** (audience) + **Morpheus** (growth strategy) |

The headline simplifications:

- **Function managers become skills.** Content and community are no longer seats; they
  are work that flows through the cognitive agents. This is the biggest conceptual shift.
- **Time gets its own axis.** ikigai-team had no dedicated "past" or "future" agent —
  review and strategy were bolted onto ops and North. Matrix promotes **Trinity** (past)
  and **Morpheus** (future) to first-class, so retrospection and foresight can't rot
  inside an execution agent's backlog.
- **Governance splits IQ / EQ.** ikigai-team split *execution* (Forge) from *care*
  (Sage) but left "is this well-governed?" implicit. Matrix makes it explicit and dual:
  **Architect** (is it structurally sound?) and **Oracle** (is it humanly sound?).

## What carries over unchanged

- `rules/agent-operations.md`, `agent-protocols.md`, `agent-proactivity.md` — sociocracy
  domain-authority, review-ready message format, default-consent time-boxes, digest
  discipline. (Language referring to "managers" now means the seven agents.)
- `rules/tasks-control-center.md` — GitHub-issues-as-queue, S3 bodies, board columns,
  the presentation-layer-vs-system-queue distinction, the daily cap.
- `rules/daily-review.md` — the heartbeat: daily/weekly review, 09:00 consent gate,
  21:00 closure. (Ownership: **Neo** runs it, **Trinity** supplies the retrospective.)
- `rules/telegram.md`, `contacts.md`, `content-publishing.md`, `orgs-projects.md`,
  `skills.md`, `shortcuts.md`, `general.md` — carried verbatim.

## What still needs a pass (v0 debt)

- ~~`rules/agent-protocols.md` / `agent-proactivity.md` still use the six-role framing.~~
  **Done** — re-voiced to the seven agents; "manager" now explicitly means any of the seven.
- ~~Cadence assignments (who wakes when) per agent.~~ **Done** — every agent file has a
  `## Cadence` section (When / What / Threshold / Output).
- No live instance is wired to Matrix yet. Cutover of `~/Orgs/ikigai` (repoint its import
  from ikigai-team to Matrix) is a separate, deliberate step — not done.
