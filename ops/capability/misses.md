# Capability Misses

The rot detector for [`workbook.md`](workbook.md). Append-only log of every time the
capability check found nothing usable.

Governed by [`rules/capability-retrieval.md`](../../rules/capability-retrieval.md).
Written by **any agent** (on declaring a gap) and by **Trinity** (on auditing an
undeclared one). Read by **Architect** (to fix rows) and **Agent** (to know what to search
the world for).

## Why this file exists

A workbook that only ever gets rows *added* rots into another unread file. This log makes
rot measurable:

- **`gap`** — no row and no skill covers the situation. Three of these on the same
  situation fires Agent's out-of-cycle world-scan; a confirmed real gap becomes a
  `skill-creator` job.
- **`unfindable`** — a row exists but the agent did not find it. **This is a workbook
  defect, not an agent defect.** Architect rewrites the row's triggers. This is the most
  valuable entry type and the easiest to under-report.
- **`unusable`** — the right skill exists and was found but could not be used (broken,
  wrong assumptions, missing config). Goes to Architect, then to `improve-skill`.
- **`undeclared`** — Trinity caught work that matched a row and did not cite it. A
  process-compliance failure, not a capability failure.

## Format

```
### YYYY-MM-DD · <type> · <situation>
Agent: <who>            Work: <what was being done>
Detail: <what was searched for, what was expected, what happened>
Row: <workbook row that should have matched, or "none">
Action: <open | fixed in <commit/row> | skill requested>
```

---

## Log

### 2026-09-08 · undeclared · full venture session used zero skills
Agent: (pre-rule session)  Work: research → strategy → website build → deploy
Detail: A complete venture session ran end to end and invoked none of 100+ available
skills. Verified by grep: `rules/skills.md` covered only *where skills are stored*; the
framework contained no retrieval obligation anywhere, and every other skill mention was a
hardcoded one-off. Specifically skipped: `website` and `frontend-design` (the build),
`web-design-guidelines` (the review), `image-to-svg` (a logo trace that was *explicitly
identified as needed* and then hand-waved), `research`, `estimation`, `user-story`,
`github-issue`.
Row: none existed — the workbook did not exist.
Action: fixed — `rules/capability-retrieval.md` created (the obligation), this workbook
seeded from these exact failures, `Skills:` added to every agent's output contract, and
the capability check added as the seventh field of the bounded-agent contract. The
`Misses` counts on the seeded rows are this incident.

### 2026-09-08 · unfindable · the word "skill" meant two different things
Agent: (framework defect)  Work: any agent reading its own config
Detail: The framework overloaded "skill" for both the bounded-contract faculty field
(`execute`, `verify`, `dispatch` — capped at 0–3) and invocable `/<skill-name>` workflows
(uncapped). An agent reading "Skills: execute · operate · dispatch" in its own config
could reasonably conclude it possessed three skills and had no business reaching for a
fourth — the bounded contract appearing to *forbid* the very retrieval this rule now
requires. A naming collision that actively discouraged the correct behaviour.
Row: n/a — terminology defect.
Action: fixed — `rules/capability-retrieval.md` separates **faculty** (bounded) from
**skill** (unbounded) and states explicitly that faculties are executed *through* skills,
so skill usage never competes for the contract's budget.

### 2026-09-08 · gap · filing a new task into the Control Center
Agent: Architect  Work: seeding this workbook
Detail: `rules/tasks-control-center.md` requires every task to be a Linear issue carrying
an S3 body (Tension · Driver · Requirement · Response Options) and added to the Control
Center. No skill automates this. The closest, `github-issue`, implements an *existing*
GitHub issue and does not file new work — and since the framework moved task tracking from
GitHub Issues to Linear, pointing new work at it would be actively wrong. Found while
reconciling the workbook after the Linear migration landed mid-session.
Row: "File a new task into the Control Center" — added, marked `gap`.
Action: open — candidate for a new skill (S3 body template + Linear create + board add).
Note the Linear MCP server requires authorisation before any such skill could run.

