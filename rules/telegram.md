## Telegram

### What Telegram is

> **Telegram is a thin client over Linear — a view and a controller, never a model.**

Every consequence in this file follows from that one sentence:

- **It holds no state.** Anything worth remembering is written to Linear or to `ops/`
  before the message is sent. A message that is the only record of something is a bug.
- **Everything it shows is rendered from a query**, not assembled from what agents happen
  to remember.
- **Everything it accepts is written through to Linear** and read back before it is
  acknowledged.
- **If Telegram and Linear disagree, Linear is right.** The surface is disposable; the
  Commander could move to any other surface tomorrow and only this file would change.

It is both a notification surface and a control surface, and it must be both: the
Commander works from Telegram and the desktop app, not a terminal. What it must never be
is a *system of record*.

Consent, the reply vocabulary, the dispatch gate, and verified writes are specified once in
[`consent-and-control.md`](consent-and-control.md). This file specifies only the surface.

### One bot, many voices

The framework ships **a single Commander bot**. Agent identity is carried by the agent
prefix in the message body (`<b>Neo:</b> …`), not by a separate bot per agent — a
per-agent bot roster is per-instance infrastructure at most, never a framework assumption.
The send path takes `--agent <name>` to select the **voice**, not a distinct bot.

Bots respond only to the Commander's own Telegram ID. Handles, tokens, chat IDs and the
send-script path are per-instance config and live in the instance's private CLAUDE.md.

---

### The three lanes

| Lane | Direction | Trigger | Writes |
|---|---|---|---|
| **Capture** | Commander → system | Any inbound message that is not bound to an issue | Linear `Triage`, or `ops/inbox/`, or a CLAUDE.md |
| **Notify** | System → Commander | The daily digest; a closed list of events | Nothing but the binding comment |
| **Control** | Commander → system → Commander | An inbound message bound to an issue | Exactly one verified transition |

An inbound message is **control** if it is a reply to a bound outbound message or names an
issue identifier; otherwise it is **capture**. There is no third case, and the test runs
before any interpretation of the words.

#### Lane 1 — Capture (in)

**Trigger.** Any unbound inbound message, in any language, of any length.

**Format.** Freeform. Capture must cost the Commander nothing; a capture path with a form
in front of it is a capture path that stops being used.

**Routing.** Split a multi-task message into its distinct items first, then route each:

| The item is | Destination |
|---|---|
| Actionable work | A Linear issue in **`Triage`** |
| A rule | The appropriate CLAUDE.md, per the `rule:` shortcut in `shortcuts.md` |
| Anything else — idea, reference, link, thought | One entry in `ops/inbox/YYYY-MM-DD-HH-MM.md` with the Telegram message id |

**The issue body at capture.** Every issue needs an S3 body
(`tasks-control-center.md`), and a captured one-liner does not have one. Resolve it by
filling what is true and marking the rest honestly:

```
Tension: <the Commander's message, verbatim>
Driver: — (unclarified at capture)
Requirement: — (unclarified at capture)
Response Options: — (unclarified at capture)
```

The remaining fields are completed on the way out of `Triage`, by whoever moves the
issue. Since capture always lands in `Triage` and `Triage` is never dispatched, the
invariant holds: **nothing reaches `Todo` with an unclarified body.** Set the project only
when the message actually says which one — an unset project is precisely what `Triage`
is for. Never guess it.

**Acknowledgement.** React once with a receipt intent (below), then reply with one line per
item created: identifier, title, state. The identifier is what makes the capture
controllable later.

**Capture must never:** create an issue in any state but `Triage`; dispatch anything; ask a
clarifying question before creating the issue (create it, *then* ask); or report a created
issue from a write response instead of a read-back.

#### Lane 2 — Notify (out)

**Trigger.** The daily digest, plus the closed event list under "Anti-noise" below.
Nothing else.

**Format.** Review-ready per [`agent-protocols.md`](agent-protocols.md) — Why · What ·
Media · Asking — rendered in Telegram HTML.

**Writes to Linear.** One comment per issue mentioned, carrying the outbound Telegram
message id, so a later reply can bind (`consent-and-control.md`). Nothing else. Notify
never changes a state.

**Notify must never:** send a half-baked update ("started on X", "still working", "FYI");
send an ask without a linked artifact when one exists; send anything a query could not
reproduce; or send at all when the query comes back empty.

#### Lane 3 — Control (bidirectional)

**Trigger.** A bound inbound message.

**Process.** Read the issue's current state → match the verb → apply at most one
transition → read back → acknowledge with the verified new state. The verb table, the
stale-reply rules, and the retry policy are in
[`consent-and-control.md`](consent-and-control.md).

**Control must never:** apply more than one transition per message; infer a verb that is
not in the table (post the message as a comment instead); write `Todo` on anything but an
explicit approval verb from the Commander; or acknowledge a transition it has not read
back.

---

### The daily digest

The digest is **a query, not an assembly.** Seven agents no longer each submit a slot —
that shape was necessary when state lived in agents' heads and is obsolete now that it
lives in one queryable place. One query, one message, three questions.

**The query** — scoped to the Commander's team:

| Section | Query | Order |
|---|---|---|
| **Decide** — what needs my decision | `state = Triage` | oldest first |
| **On you** — what is blocked on me | `label = agent:commander` AND `state ∈ {Backlog, Todo, In Progress}` | oldest first |
| **Shipped** — what shipped | `state = Done` AND `updatedAt >= last digest` | newest first |

Plus a one-line footer: the count of `In Progress` (work in flight, needing nothing).

"On you" is the route that `agent:commander` never had. The label marks work only the
human can do — physical-world, GUI-only, non-delegable — and without this section those
issues sit in the queue forever, since no dispatcher will ever pick them up. The label
means *this needs the Commander*, so the digest must be where it is said.

**Shape** (Telegram HTML; sections with no rows are omitted entirely):

```html
<b>Digest · 2026-09-09</b>

<b>Decide</b> (2)
<a href="...">RAZ-201</a> · Reconcile the workbook after migration
<a href="...">RAZ-202</a> · Yamblebee pricing page

<b>On you</b> (1)
<a href="...">RAZ-178</a> · Sign the lease — 4d

<b>Shipped</b> (1)
<a href="...">RAZ-190</a> · Capability retrieval rule — <a href="...">PR</a>

2 in flight. Reply to a line: ok · no · hold · done
```

**Bounds.** At most 5 rows per section; the overflow becomes one `+N more` line linking to
the Linear filter. **If all three sections are empty, no message is sent** — the digest
obeys the silent-cycle rule like any other cycle.

---

### Anti-noise

Seven agents and an hourly dispatcher can generate events all day. The DM is bounded on
purpose.

- **One scheduled outbound per day** — the digest. Silent when the query is empty. A daily
  silent cycle is a healthy cycle.
- **The dispatcher is silent.** It picks up `Todo`, works, and comments the PR back onto
  the issue. It sends **zero** messages. Its output appears in the next digest's *Shipped*
  section. The single largest event source is therefore the one that never reaches the DM.
- **Out-of-cycle sends are allowed for exactly three events**, and this list is closed:
  1. A flagged human-emergency or wellbeing signal (Oracle's lane, per `architecture.md`).
  2. A **strategic**-stakes decision that is blocking work right now.
  3. A failure the Commander must know about today — a dispatched agent failed in a way
     that loses work, or something in production is broken.
- **Ceiling: three out-of-cycle messages per day.** The fourth and beyond are merged into
  one and held for the digest, except category 1, which is never held.
- Adding a fourth event category is an Architect decision, not an agent's judgement call.

---

### Reactions are receipts, not classification

A reaction previously encoded GTD state (idea, someday, reference) — state held on the
surface, which is exactly what now belongs in Linear or `ops/inbox/`. A reaction is now a
**receipt**: it reports what the system did with the message.

| Intent | Means |
|---|---|
| Captured | Landed in Linear `Triage`; identifiers follow in the reply |
| Noted | Landed in `ops/inbox/` or a CLAUDE.md — not a task |
| Applied | A control verb was accepted and the transition was read back |
| Unclear | Ambiguous; a clarifying reply follows |
| Failed | The write did not verify after retries — needs the Commander in Linear |

React **once** per message. Telegram allows only a fixed emoji set, so the framework
specifies intents; the actual emoji is per-instance config.

### Formatting

- **Telegram-supported HTML only** — `b`, `i`, `u`, `s`, `a`, `code`, `pre`, `blockquote`,
  `tg-spoiler`. No markdown headers, bullets, or code fences; Telegram will not render
  them and the message arrives as literal syntax.
- **Always write the body to a temp file first** and pass it to the send script with
  `--file <path>`. Inline message text on the command line corrupts on multi-byte content.
- Long-form output goes to a file and the message links to it (`daily-review.md`: file
  first, message second). Chat scrollback is not durable storage.

### Telethon for advanced ops

The bot API cannot react to or edit messages — Telethon is required for reactions, edits,
and message lookups. Run it via `uvx`; session credentials live at the per-instance path.

### Standard bot commands

- `/start` — greeting
- `/reset` — clear context
- `/opus <msg>` — escalate to Opus for one message

### Real-time processing

A single listener process owns real-time message intake. Do not add Telegram processing to
other sessions — one owner, or messages get double-handled and issues get created twice.
