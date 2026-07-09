# Matrix — Framework Root

Matrix is a bounded-cognition agent operating system: a wiki of governance rules,
agent personas, and operating patterns that any human + AI commander can adopt. It is
the successor to **ikigai-team** — same proven operating knowledge, re-organised around
a cognitive-faculty agent carve instead of a function carve.

Like ikigai-team, it is designed to be **imported, not copied**. Your private brain repo
(or any org/project repo) declares a single import line referencing this file's absolute
path (e.g. `@` followed by `/Users/you/Projects/matrix/CLAUDE.md`) and inherits the full
framework. Updates land via `git pull`. No installer, no template renderer.

---

## Read the architecture first

The agent topology, the bounded-agent contract, and the router protocol are specified in
**[architecture.md](architecture.md)**. Everything below assumes it.

The seven agents: **Matrix** (router) → **Trinity** (past) · **Neo** (present) ·
**Morpheus** (future) → and under Neo: **Architect** (governance IQ) · **Agent**
(reality) · **Oracle** (governance EQ). Each is defined in `.claude/agents/`.

---

## Framework rules (imported)

@rules/agent-team.md
@rules/agent-operations.md
@rules/agent-protocols.md
@rules/agent-proactivity.md
@rules/tasks-control-center.md
@rules/daily-review.md
@rules/telegram.md
@rules/content-publishing.md
@rules/contacts.md
@rules/skills.md
@rules/orgs-projects.md
@rules/shortcuts.md
@rules/general.md

---

## What this framework does NOT include

- **Your identity.** Names, OKRs, contact lists, project paths, API keys — all live in
  your private repo that imports this one.
- **Your bot tokens.** Telegram handles and `*_BOT_TOKEN` env vars are per-instance.
- **Your instance's project/org roster.** The seven agents are timeless; the projects
  they act on are instance-side.
- **Anything time-bound.** Current campaigns, this-quarter OKRs, this-week focus — all
  instance-side. The framework is timeless.

If personal data leaks into a framework rule, that's a bug.
