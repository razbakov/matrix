## Capability Retrieval

The framework had a filing system for skills (`skills.md`) and no index. Nothing obliged
any agent to ask "does a skill already cover this?" — so nothing did. A full venture
session (research → strategy → build → deploy) ran and invoked **zero** of 100+ available
skills, including ones explicitly identified as needed. This rule is the missing
obligation.

Read [`skills.md`](skills.md) for where skills are *stored*. This file is how they get
*found and used*.

---

### Terminology — two different things called "skill"

The framework overloads the word. Fix it by name, because the collision is part of why
retrieval never happened: an agent that reads "Skills: execute · operate · dispatch" in
its own config reasonably concludes it has three skills, and never looks for `/website`.

- **Faculty** — the bounded-contract field in `.claude/agents/<name>.md`. An abstract
  capability the agent owns (`execute`, `verify`, `retrospective`). Capped at 0–3, max 12.
  These are *what the agent is*.
- **Skill** — an invocable `/<skill-name>` workflow from the shared skills repo
  (`website`, `image-to-svg`, `estimation`). Uncapped. These are *what the agent reaches
  for*. A faculty is executed **through** skills.

Faculties do not compete with skills for the bounded contract's budget. An agent with the
faculty `execute` may invoke a hundred different skills in its life and stay bounded,
because the bound is on *what it decides*, not on *what it uses*. Nothing in the
bounded-agent contract has ever limited skill usage — the appearance that it did was a
reading error this rule closes.

---

### The retrieval obligation

**Before any agent begins a unit of work, it must consult the capability workbook and
declare the result.** This is a precondition on accepting the input, not a process step —
it does not consume any agent's 0–3 process budget (see the seventh field of the
bounded-agent contract in [`architecture.md`](../architecture.md)).

The obligation in full:

> **Capability check.** Before starting any unit of work, grep
> `ops/capability/workbook.md` for the situation at hand. If a row matches, invoke the
> skills it names — do not hand-roll the workflow. If no row matches, check the live
> skill list once (`ListSkills` / the `Skill` tool's listing). Then declare the result on
> the output as `Skills:`.
>
> A miss is a legitimate outcome; an **undeclared** miss is not. `Skills: none` with no
> reason is a contract violation, and Trinity audits for it.

**"A unit of work" means every task an agent claims — including sub-tasks discovered
mid-execution.** Checking once at intake is not enough and was never the failure's cure:
the venture session was routed once and then discovered "vectorise the logo," "review the
UI," "verify the deploy" internally, long after routing. Each of those is its own unit of
work and each owed its own check.

### Two insertion points, both mandatory

| Point | Who | When |
|---|---|---|
| **Intake gate** | Matrix, at **Ownership** (GROW+Owner step 5) | Once per inbound request. Route *and* name the covering skills, so the owner starts with them. |
| **Execution gate** | Every agent, on accepting any unit of work | Every task and every discovered sub-task. |

Matrix's check is not a substitute for the execution gate. Matrix sees the request; only
the executing agent sees the sub-tasks the request decomposes into. Both fire.

### Declaring the result

Every agent's output format carries a `Skills:` line. Three legal values:

```
Skills: website, frontend-design          # invoked, per workbook row "build a website"
Skills: none — no workbook row, no match in the live skill list for <situation>
Skills: none — <skill> covers this but was unusable: <specific reason>
```

The third form is a **miss** and must also be logged to
`ops/capability/misses.md` (see below). The second form is a **gap** and is logged the
same way.

### Skills that are already mandatory

Some skills are not merely available — an existing rule already requires them. These are
non-negotiable regardless of workbook state:

- `image-from-gemini` for hero images and YouTube thumbnails
  (`rules/content-publishing.md`).
- `skill-creator` before publishing any new skill (`rules/skills.md`).
- `meal-suggestion` for the daily meal plan (`rules/daily-review.md`).
- `org-coach` when creating an organisation (`rules/orgs-projects.md`).

---

### The workbook

Canonical location: **`ops/capability/workbook.md`**. State lives in the repo, never in
agent memory (`rules/agent-operations.md`).

- The **framework** ships the shared table — situations that any instance faces, mapped to
  skills that are shareable by rule.
- An **instance** may overlay `ops/capability/workbook.md` in its own repo with rows for
  its private skills. On conflict, the instance row wins.
- The workbook is **one file, greppable, one row per situation.** This is a hard design
  constraint. A capability index that costs more than one grep will be skipped, exactly as
  the raw skill list was.

Row shape:

| Situation | Skills (in order) | Owning agent | Last used | Misses |

`Last used` and `Misses` are the rot detector — see below.

### Who owns the workbook

**Architect.** Under S3:

- **Domain** — the framework's capability map: which situation is served by which skill
  and which agent. This is domain carving at task granularity, and domain carving is
  already Architect's `architecture` faculty. It is not a new faculty and not a new agent.
- **Driver** — every situation the system faces has a known, findable, correct owner and
  workflow, so no agent hand-rolls work a skill already does.
- **Accountability** — keeps every row correct and findable; adds rows for new
  capabilities Agent reports; fixes or prunes rows Trinity reports as missed or dead;
  answers to Neo for workbook health.

---

### The feedback loop

Three existing agents, three existing faculties, zero new roles.

```
   Agent  ──(inventory: what capability exists)──▶  Architect ──▶  workbook
   Trinity ──(misses: what should have been used)──▶     │
                                                          ▼
                                     every agent + Matrix consult it
```

**Agent — capability inventory** (existing `data` faculty; a new source, not a new
faculty).

- **Domain** — ground truth, unchanged. "What skills exist right now, locally and in the
  world" is a factual question about current state, which is Agent's entire remit.
- **Driver** — the workbook describes capability that actually exists, not capability
  someone remembers existing.
- **Accountability** — reports the current skill inventory (installed sources + repo) and
  candidate new/updated skills from the outside world, as facts with sources, up to
  Architect.
- **Cadence** — **monthly**, first working day: scan installed skill sources for
  added/removed/renamed skills; search the outside world for skills covering situations
  that `misses.md` shows as uncovered. **Event trigger:** three or more misses logged
  against the same uncovered situation fires an out-of-cycle scan — that is a real
  capability gap and waiting a month costs real work.
- **Threshold** — reports only deltas. An unchanged inventory logs "nothing to surface."

**Trinity — miss detection** (existing `retrospective` faculty).

- **Domain** — the past, unchanged. "Work was done without the skill that covered it" is a
  plan-vs-actual finding, which is precisely Trinity's remit.
- **Driver** — a skipped skill is detected instead of silently repeating.
- **Accountability** — audits completed work for undeclared or wrong `Skills:` lines,
  appends findings to `ops/capability/misses.md`, and hands the row-level diagnosis to
  Architect.
- **Cadence** — **daily**, inside the existing end-of-day retrospective (no new cycle):
  grep the day's agent outputs for `Skills: none` and for work matching a workbook row
  that did not cite it. **Weekly**, inside the existing weekly review: report workbook
  health — rows unused 90+ days, rows with repeat misses, situations with no row.
- **Threshold** — a single declared gap is normal and silent. Surfaces a *pattern*: the
  same situation missed twice, or a workbook row that exists but keeps being missed
  (meaning it is worded so it cannot be found).

**Architect — curation** (existing `architecture` faculty).

- **Cadence** — **event-triggered**, per its existing reactive profile: wakes on an Agent
  inventory delta or a Trinity miss report. **Plus monthly**, on receipt of Agent's scan,
  to reconcile the workbook against the real inventory.
- **Threshold** — edits the workbook silently (routine, inside its domain authority);
  surfaces to Neo only when a gap needs a *new skill written*, which is a Commander
  decision about where to spend effort.

Direction is preserved: Agent and Trinity are peers under Neo and Matrix respectively and
do **not** speak sideways to Architect as a free-for-all. Inventory and miss reports go
**up** as reports and are picked up by Architect through Neo, per
`rules/agent-team.md`'s delegation direction. In practice Neo forwards; the loop is not a
new edge in the topology.

---

### What forces this to actually run

The honest answer, stated plainly, because "we wrote a rule" is what failed last time. Four
layers, weakest to strongest:

1. **Loaded by default.** This file is in `CLAUDE.md`'s import list, so it is in context
   for every session. The previous failure had no such rule in any loaded file. Necessary,
   not sufficient — an in-context instruction can still be ignored.
2. **A contract field, not an instruction.** The capability check is the seventh field of
   the bounded-agent contract, alongside "Input format: 1." Contract terms are structural;
   buried instructions are advisory. Stronger, still soft.
3. **A required output field.** `Skills:` must be filled on every agent's output. This is
   the load-bearing soft layer, because it converts an omission into an **artifact**. You
   cannot silently skip a field you must fill; skipping it produces visible evidence of
   skipping it. An agent can still lie, but it must lie in writing.
4. **Mechanical audit.** Trinity greps completed work for `Skills: none` and for
   workbook-matching work that did not cite its row. This is the only layer that does not
   depend on an agent's good faith, and it is therefore the real enforcement. It closes the
   loop the previous design left open: the failure gets *detected* rather than repeating
   invisibly.

**Layer 4 is the mechanism.** Layers 1–3 make compliance cheap and non-compliance
visible; layer 4 makes non-compliance *caught*. A design that stops at layer 1 is the bug
this rule exists to fix.

### What keeps the workbook from rotting

A workbook nobody reads is the same bug in a new place. Four countermeasures:

- **Near-zero cost to read.** One file, one grep, one row. Cost is the whole reason the
  raw skill list went unread.
- **Reading is observable.** Consulting it produces the `Skills:` line; not consulting it
  produces a visible empty one.
- **Rot is measured, not assumed.** Each row carries `Last used` and `Misses`. A row
  unused for 90 days is either wrong or describes a situation that never occurs —
  Architect prunes it. A row with repeat misses exists but cannot be found — Architect
  rewrites its trigger wording. Rot becomes a number instead of a vibe.
- **Health is on a standing agenda.** Trinity's weekly review carries a workbook-health
  line. A metric with no scheduled reader is not a metric.
