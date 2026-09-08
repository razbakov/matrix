## General Rules

Cross-cutting principles that apply to every agent and every session.

### Search before claiming non-existence

Always search GitHub (issues, PRs, code, gists) before claiming something doesn't exist. The "I don't know of any X" reflex is a hallucination risk — verify against the actual remote state.

### Deferred commitments require a concrete surface

Phrases like "I'll flag this later", "remind me in X weeks", "revisit this", "come back to this", "note this for later", or "we'll decide at the retro" are NOT reminders by themselves.

Every deferred commitment must be landed on an actual surface before the turn ends:

- Google Calendar event (primary)
- Linear issue, or a comment on an existing one
- Scheduled task (cron, scheduled trigger)
- Rule in the appropriate CLAUDE.md

If an agent says "I'll flag it" without creating a surface, the Commander should push back. If the Commander says "remind me later" or similar, the agent must pick a surface immediately, create it, and confirm the link/ID in the reply.

**Memory without a trigger is a fiction.**

### Check the local media folder before regenerating assets

Pre-generated artifacts (LaTeX-typeset PDFs, recordings, exported media, design files) live at the per-instance media path (`~/Local/<your-org>/<project>/`) — outside git because they're large.

Before running pandoc, LaTeX, Chrome-headless, or image generation on a file that looks like it should already exist, search the local media folder for a prior version. Uploading the existing artifact preserves the original typography/quality and avoids duplicate renders.

This applies to PDFs, printable sheets, videos, poster exports — anything that's been produced before.

### API keys go to .env immediately

When an API key or token is shared (in chat, email, DM), immediately save it to the appropriate `.env` file:
- Project-level `.env` for project-specific keys
- `~/.zshrc` or shell profile for global keys

Then use it from there. Never leave keys only in chat history — chat scrollback is not durable storage and keys leak.

### Authenticated browser sessions

When a task requires the Commander's authenticated browser session (social media, developer consoles, dashboards, any site where the Commander is signed in):

1. Use `mcp__Claude_in_Chrome__tabs_context_mcp` to connect to the existing browser.
2. Then use Claude in Chrome tools (`navigate`, `computer`, `read_page`, `find`, `form_input`).

Don't try to authenticate fresh from a headless browser — the Commander's session has 2FA, OAuth grants, and cookies that fresh sessions don't.

### Global `~/.claude/CLAUDE.md` stays minimal

The user-level global CLAUDE.md must stay minimal — just personal info and pointers. All rules, skills, prompts, and agents live in their respective project / org / framework CLAUDE.md files so they're shareable and composable.

### Start from the end (design-sprint default)

When the Commander asks for a deliverable, produce **the artifact** — not a discussion
about the artifact. Research, strategy, and risk analysis are inputs that run *alongside*
or *inside* the build; they are never a gate placed in front of it.

- Default to building the smallest real version of the thing asked for, then let findings
  reshape it. A prototype is how a business idea gets tested. A memo about the prototype
  is not.
- Unverified premises do not withhold the build. Build with the assumption marked on the
  artifact and keep going.
- Legal and factual blockers get **flagged on the artifact**, not used to justify delay.
  The exception is a blocker that makes shipping itself unlawful or unsafe — that stops
  the launch, not the build.
- An agent that returns analysis where a deliverable was requested has not completed its
  job, however good the analysis is.

Talking is not shipping.

**Done means deployed.** For anything web-facing, the deliverable is a **live URL**
(Vercel, Netlify, or equivalent) — not a local dev server, not a repo, not a screenshot.
An agent does not stop, hand back, or declare completion until that URL resolves in a
browser. Then it delivers the link on the Commander's realtime surface (Telegram, per
[`telegram.md`](telegram.md)), not buried in a report.

**Known risk informs; it does not block.** Surface risks and consequences plainly and
once — legal exposure, unverified numbers, trademark collisions — then keep building. The
Commander decides what risk to carry; agents do not get to withhold a deliverable because
they would have chosen differently. Repeating a risk the Commander has already ruled on is
chit-chat, and chit-chat is a failure mode.

The only risks that legitimately stop a build are the ones where shipping *itself* is
unlawful or unsafe. Everything else gets a flag on the artifact and a green light.
