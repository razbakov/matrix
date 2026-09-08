# Capability Workbook

**Situation → skills → owning agent.** The framework's index of what already exists, so
no agent hand-rolls work a skill already does.

Governed by [`rules/capability-retrieval.md`](../../rules/capability-retrieval.md).
Owner: **Architect**. Fed by **Agent** (inventory) and **Trinity** (misses).

## How to use this file

1. **Grep it for your situation before starting any unit of work.** One grep. That is the
   whole cost, and keeping it that low is a design constraint.
2. Match a row → **invoke the skills it names, in order.** Do not re-derive the workflow.
3. No match → check the live skill list once (`ListSkills`), then proceed.
4. **Declare the result** on your output as `Skills:` — see the retrieval rule. Log a gap
   or an unusable skill to [`misses.md`](misses.md).

Rows are matched on the **Situation** and **Triggers** columns; triggers exist so the row
is findable by the words an agent would actually use. If you had the situation and did not
find the row, that is a miss — log it, so Architect can fix the wording.

`Last used` / `Misses` are the rot detector. A row unused 90+ days gets pruned; a row with
repeat misses gets rewritten.

---

## Build & ship

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Build a website or landing page | website, landing page, marketing site, homepage, web build | `website` (methodology: content-first, mobile-first) → `frontend-design` (production-grade UI) | Neo | — | 1 |
| Build a web component / dashboard / app UI | component, dashboard, React, UI, styling, "make it look good" | `frontend-design` | Neo | — | — |
| Review UI quality / accessibility / UX | review my UI, check accessibility, audit design, review UX, design review | `web-design-guidelines` | Architect | — | 1 |
| Check visual fidelity against a design | does it match, visual parity, pixel check | `visual-parity-audit` | Architect | — | — |
| Implement from a Figma file | figma, node id, implement design | `figma-implement-design` | Neo | — | — |
| Review code for correctness | code review, review the diff, check this PR | `code-review` (built-in) | Architect | — | — |
| Address PR review comments | fix PR comments, address the review, review feedback | `pr-review-responder`; all open PRs → `review-all-prs` | Neo | — | — |
| Web performance / Lighthouse | slow site, performance, LCP, lighthouse | `web-perf`; deep trace → `chrome-devtools-mcp:debug-optimize-lcp` | Architect | — | — |
| SEO audit of a site | seo, ranking, meta tags, sitemap | `seo-audit`; Nuxt → `nuxt-seo-audit`; schema → `schema-markup` | Neo | — | — |

## Verification & ground truth

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| **Verify a subagent's deploy claim** | "it's deployed", done, shipped, live URL | *Process, not a skill* — see below | Agent | — | 1 |
| Drive a browser / automate a web task | click, fill form, scrape, log in, screenshot a site | `use-browser` (meta — routes to the right toolset) | Agent | — | — |
| Check analytics / DAU / traffic | DAU, pageviews, analytics, traffic, PostHog | `analytics-check` | Agent | — | — |
| Check dependency vulnerabilities | vulns, CVE, audit dependencies | `dependency-vuln-report` | Agent | — | — |
| Security review of a change | security review, is this safe, injection | `security-review` | Architect | — | — |

**Verify a deploy claim** has no skill and needs none — it is a three-step process Agent
runs under its `verify` faculty, mandated by "Done means deployed" in `rules/general.md`:

1. Fetch the claimed live URL (`WebFetch`, or `use-browser` if it is behind auth/JS).
2. Assert HTTP 200 **and** that the expected content is actually present — a 200 on a
   generic host placeholder is a failed deploy, not a passed one.
3. Report `Fact:` with the URL, the status, and the freshness stamp.

A deploy claim with no fetched URL behind it is unverified. Report it as
`could-not-establish`, never as done.

## Research, strategy & planning

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Research a topic and document it | research, investigate, look into, find out about | `research` (saves to `research/yyyy-mm-dd-title.md`) | Morpheus (strategic) / Agent (factual) | — | 1 |
| Validate a product idea / find the right problem | should we build, is this a good idea, validate, discovery | `product-coach` | Morpheus | — | — |
| Plan a launch | launch, go to market, GTM | `launch-strategy` | Morpheus | — | — |
| Set or check pricing | pricing, how much should we charge, price point | `pricing-strategy` | Morpheus | — | — |
| Survey competitors / alternatives | competitors, alternatives, who else does this | `competitor-alternatives` | Morpheus | — | — |
| Run a design sprint | design sprint, rapid prototype week | `design-sprint` | Morpheus | — | — |
| Start a new project properly | new project, kickoff, project setup, onboarding to a repo | `project-start` | Neo | — | — |
| Design org governance / roles / domains | organisation, governance, S3, sociocracy, who decides | `org-coach` (**mandatory** per `rules/orgs-projects.md`) | Architect | — | — |

## Work management

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Estimate effort / size a piece of work | estimate, how long, story points, size this | `estimation` | Matrix (owns "estimate") | — | 1 |
| Write a user story / acceptance criteria | user story, acceptance criteria, requirements, ticket | `user-story` | Neo | — | 1 |
| File a new task into the Control Center | new task, track this, add to the board, file an issue | **No skill covers this** — follow `rules/tasks-control-center.md`: Linear issue with an S3 body (Tension · Driver · Requirement · Response Options) | Neo | — | gap |
| Capture a Telegram message into the queue | telegram inbox, capture this, split into issues, process the DM | **No skill covers this** — follow `rules/telegram.md` lane 1: split, route (issue → `Triage` / rule → CLAUDE.md / other → `ops/inbox/`), read-back verify | Neo | 2026-09-09 | gap |
| Act on a Commander reply / approve or park work | ok, ship it, hold, park it, approve, what do these replies do | **No skill** — the verb → Linear transition table in `rules/consent-and-control.md`; read current state first, read back after | Neo | 2026-09-09 | gap |
| Send the daily digest | digest, daily ping, what needs my decision, what shipped | **No skill** — the three-section Linear query + message shape in `rules/telegram.md`; silent when empty | Matrix (owns "plan communication") | 2026-09-09 | gap |
| Implement a given GitHub issue | implement issue, this issue URL, work on #N | `github-issue`; pick next → `github-next-issue` | Neo | — | 1 |
| Plan a sprint | sprint planning, what should we build next, plan the iteration | `sprint-planning`; dispatch it → `run-sprint`; close it → `sprint-release` | Neo | — | — |
| Dispatch a fire-and-forget agent | inbox, dispatch, spin up an agent | `inbox` | Neo | — | — |
| Check on dispatched agents | scrum, agent status, what's running | `scrum` | Neo | — | — |
| Orchestrate a complex multi-step task | plan this, coordinate subagents, verify the work | `workflow`; specs → `writing-plans` | Neo | — | — |
| Write BDD scenarios | BDD, gherkin, given/when/then, feature file | `bdd-scenarios`; from an existing UI → `bdd-from-ux` | Neo | — | — |
| Test-first implementation | TDD, write tests first, red-green | `test-driven-development` | Neo | — | — |

## Visual assets

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| **Vectorise a logo / raster → SVG** | vectorise, vectorize, trace a bitmap, logo to SVG, PNG to vector | `image-to-svg` (Potrace) | Neo | — | 1 |
| Design a logo from scratch | logo concepts, brand mark, logo options | `logo-generator` | Neo | — | — |
| Hero image for a post / YouTube thumbnail | hero image, thumbnail, article image | `image-from-gemini` (**mandatory** per `rules/content-publishing.md`) | Neo | — | — |
| Poster / flyer / print or social graphic | poster, flyer, print, social graphic | `brand-poster`; code-first → `image-from-html`; typographic/vector → `image-from-latex` | Neo | — | — |
| Charts, graphs, dashboards, any data viz | chart, graph, plot, dashboard, visualise data | `dataviz` (**read before writing chart code**) | Neo | — | — |
| Multi-artboard mockups / wireframes | mockup, wireframe, screen flow, design canvas | `design` | Neo | — | — |
| QR code with tracking | QR code, UTM QR | `qr-code-generator` | Neo | — | — |
| Generate a video / animate a still | video, clip, animate this image | `video-from-gemini` | Neo | — | — |

## Content & publishing

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Write or edit marketing copy | copy, headline, landing copy, rewrite this | `copywriting` → `copy-editing`; psychology → `marketing-psychology` | Neo | — | — |
| Post to X / social | tweet, thread, X post, social post | `x-post`; cross-platform → `social-post` | Neo | — | — |
| Publish a page a team will read or use | report, memo, plan, one-pager, shareable doc | Artifact tool (+ `artifact-design`) | Neo | — | — |
| Produce a PDF / printable report | PDF, printable, typeset report | `latex-pdf` | Neo | — | — |
| Process a recording into clips + metadata | cut this recording, make clips, shorts, podcast post-production | `video-podcast-producer`; transcript only → `transcribe-via-faster-whisper` | Neo | — | — |
| Update YouTube metadata | youtube title, description, chapters, tags | `youtube-metadata-updater` | Neo | — | — |
| Run an A/B test | A/B test, split test, experiment, variant | `ab-test-setup` | Morpheus | — | — |

## Operating cadence

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Daily review | daily review, morning routine, plan my day | `daily-review` (meta — orchestrates the children) | Neo | — | — |
| Meal plan for the day | what should I eat, lunch, dinner, meal plan | `meal-suggestion` (**mandatory** per `rules/daily-review.md`) | Neo | — | — |
| Weekly review | weekly review, weekly planning, Saturday review | `weekly-review`; annual → `year-review` | Trinity | — | — |
| Daily standup DM | standup, morning DM | `daily-standup` | Neo | — | — |
| Process the inbox | inbox, saved messages, GTD, clarify | `process-inbox` | Neo | — | — |
| Export / share an AI chat session | export chat, share this session, transcript of our work | `export-chat-history` | Trinity | — | — |
| Recommend unused skills from history | what skills should I use, unused skills | `history-skill-recommender` | Architect | — | — |

## Meta — the skills system itself

| Situation | Triggers | Skills (in order) | Owner | Last used | Misses |
|---|---|---|---|---|---|
| Create a new skill | new skill, make a skill, capture this workflow | `skill-creator` (**mandatory review** per `rules/skills.md`) → publish to the skills repo | Architect | — | — |
| Improve an existing skill | improve skill, fix triggering, optimise this skill | `improve-skill`; standards → `writing-skills` | Architect | — | — |
| Rewrite or reconcile a framework rule | rule file, framework rule, rules contradict, governance redesign | **No skill covers this** — Architect's `architecture` faculty applied directly. `org-coach` is scoped to *creating an organisation* and does not fit. Check every importer of the rule for contradictions before committing. | Architect | 2026-09-09 | gap |
| Split a fat skill into children | decompose skill, meta-skill, break this up | `meta-skill` | Architect | — | — |

---

## The composite row: a full venture session

This is the exact chain the failed session should have run. It is recorded as a row
because the failure was not one missed skill — it was a missed *chain*, and each link
was a separately discovered sub-task that owed its own capability check.

| # | Sub-task | Skill | Owner |
|---|---|---|---|
| 1 | Research the market and the problem | `research`, `competitor-alternatives` | Morpheus |
| 2 | Validate the idea, choose the direction | `product-coach`, `launch-strategy` | Morpheus |
| 3 | Size and sequence the work | `estimation`, `user-story`, then file to Linear per `rules/tasks-control-center.md` | Matrix → Neo |
| 4 | Name and mark the brand | `logo-generator`, then `image-to-svg` to vectorise | Neo |
| 5 | Build the site | `website` → `frontend-design` | Neo |
| 6 | Review before shipping | `web-design-guidelines`, `code-review` | Architect |
| 7 | Deploy and **verify the live URL** | *deploy-verification process, above* | Agent |
| 8 | Deliver the link on the realtime surface | `rules/telegram.md` | Matrix (comms) |

Step 7 is not optional. `rules/general.md`: **done means deployed**, and a deploy is not
deployed until someone fetched the URL.
