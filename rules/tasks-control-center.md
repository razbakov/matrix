## Tasks & Control Center

How work gets tracked across all your projects without dropping anything.

### All tasks live as Linear issues

- All tasks live as **Linear issues** (the "Control Center"). Never add task items to markdown files. Never use GitHub Issues or Notion for tasks.
- Each task issue is created on the **team that owns the work**, labeled with one `agent:*` label, and carries the project/repo it belongs to (matching the Project Path Registry in your private CLAUDE.md). Team keys, project IDs, and the API key env var are per-instance config — see your private CLAUDE.md, not this file.
- Every task issue must have an **S3 body** (Sociocracy 3.0):
  - **Tension** — what hurts or feels off
  - **Driver** — the underlying need driving this work
  - **Requirement** — what success looks like
  - **Response Options** — possible ways forward

### Workflow states

`Triage → Backlog → Todo → In Progress → Done`

The exact state names are per-instance — read them from the team before assuming.
Not every workspace has a review state; where one is missing, "ready for review"
is carried by the linked PR, not by an issue state.

- Don't hand an issue over for review if it or its PR has unresolved threads.
- "In Review" means deliverables are in the PR — the PR body must link to every artifact (files, URLs, deployed preview).
- Link the PR to the issue so it closes on merge: put the issue identifier in the **branch name** (`<identifier>-short-slug`) or a magic word in the PR body/title (`Fixes ENG-123`). Linear's GitHub integration then advances the state automatically. A PR with no issue identifier anywhere is invisible work.

### Tasks vs personal action items

- **Linear issues** (Control Center) = delegated or cross-session work. Anything an agent picks up. Anything that needs an S3 body, a PR, a deliverable. **This is the system queue.**
- **Google Tasks** = personal actions only the Commander does (call, send, buy, read). No agent involvement, no PR, no deliverable. **This is a presentation layer, not a queue** (see next section).

If a task needs an agent, it's a Linear issue. Never a Google Task.

### Tasks/Calendar are a presentation layer, not a system queue

The Commander is human; they can hold ~5 commitments per day. The team is not human; it can hold infinite parallel state. Therefore:

- **Google Tasks and Google Calendar** are *today's curated slice* the Commander has explicitly consented to. They are the presentation layer between the system and the human.
- **The system queue is Linear + `ops/`.** Anything the team is tracking, anything in flight, anything not yet consented-to lives there.
- **A hard daily cap on actionable items** (calendar blocks + tasks combined, excluding sleep/meals/standing meetings) prevents flooding. The cap is per-Commander config in private CLAUDE.md; default suggestion: 5.
- **Items only enter Tasks/Calendar via the daily consent gate** (see `daily-review.md`). No agent, no skill, no daily-review process may auto-add items — they may only *propose* during the morning consent step.
- **Items removed at end-of-day go back to the system queue**, not to a hidden personal backlog. The next day's consent gate re-competes for the cap slots. No silent backlog growth.

This rule fixes the failure mode where Tasks becomes a dumping ground for everything anyone (the Commander, agents, daily-review, follow-ups) thinks should happen, until the volume guarantees nothing gets done. In sociocracy terms: the Commander cannot have objections to items they never consented to, and labeling unconsented items "missed follow-ups" against any reliability KR is a category error — the leak is on the team, not on the Commander.
