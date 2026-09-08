## Consent & Control

The single ruling on how work advances, stated once so every surface obeys it — Linear,
Telegram, the daily review, and any surface added later. Where another rule appears to
say otherwise, this file wins.

### The ruling

> **Silence never advances work. Silence may only park it.**

Consent is an act, never an absence. No agent, cycle, skill, or time-box may move work
forward on the grounds that the Commander did not object. Where the Commander says
nothing, the default direction is always toward *less* action, never more.

### Why this replaced default-consent time-boxes

The 4h/24h "silence = ship" time-box was correct for the system that had it. Back then the
ask existed **only as a chat message**: if silence meant nothing, the ask scrolled away and
the work died. Assigning silence a meaning was the only way to keep the queue moving.

That premise is gone. The ask now lives in Linear as a durable issue in `Triage`. It does
not scroll away, it does not expire, and it re-appears in every digest until the Commander
touches it. Silence no longer needs to be assigned a meaning, because nothing is lost by
it — so it gets the safe meaning instead of the convenient one.

This also dissolves the "different consent rules for different risk classes" question. You
do not need risk classes when the default direction is always de-escalating. Risk class
survives, but as a **volume** control (how loudly an item is surfaced), never as an
**authority** control (whether it may proceed unasked). See "Stakes → volume" below.

### The dispatch gate

**An issue in `Todo` is approved.** That is the whole gate — one state, no label, no
second signal.

- The scheduled dispatcher picks up `Todo`, locks the issue to `In Progress`, hands it to
  Matrix for routing, and comments the resulting PR back onto the issue.
- **Only the Commander puts an issue into `Todo`** — in Linear directly, or through a
  control verb below. No agent, cycle, or skill may write `Todo`.
- Every other state is safe: an agent may create, refine, comment on, re-project, and
  re-body an issue in `Triage`, `Backlog`, or `In Progress` without asking. That is domain
  authority, and it is unrestricted precisely because none of it dispatches anything.
- **No issue reaches `Todo` with an unclarified S3 body.** Whoever moves it completes
  Driver · Requirement · Response Options first (see `tasks-control-center.md`).

Read that as the sociocracy shape it is: agents have full authority inside their domain,
and exactly one act — committing the Commander's attention and the system's compute — is
reserved.

### The reply vocabulary

Every verb the Commander can say maps to exactly one Linear transition, or to explicitly
nothing. There are no other verbs; anything unlisted is a comment, not a command.

| Verb | Precondition | Effect | If the precondition is unmet |
|---|---|---|---|
| `ok` · `yes` · `ship it` · `go` | state ∈ {Triage, Backlog} | → **Todo** (arms the dispatcher) | Already Todo/In Progress → no-op, ack "already armed". Done → no-op. |
| `no` · `not now` · `park` | state ∈ {Triage, Todo} | → **Backlog**, plus a comment carrying the Commander's words | Backlog → no-op. In Progress → comment only; stopping running work is done in Linear. |
| `hold` · `pause` | any state before Done | Stays put, except **Todo → Backlog** (un-arms the gate). Comment `hold until <date>`, default +7d. Suppressed from the digest until that date. | — |
| `done` | issue carries `agent:commander` | → **Done** | Without the label → no transition; reply "agent work closes on the merged PR". |
| `mine` · `I'll do it` | any state before Done | Add `agent:commander`, → **Backlog** | — |
| `focus: <theme>` | — | **No Linear write.** Writes `ops/agents/matrix/focus.md`; re-sorts every agent's next cycle and the next digest. | — |
| anything else | bound to an issue | **No transition.** Posted verbatim as a Linear comment. | Not bound to an issue → it is capture, not control (see `telegram.md`). |

**No verb, use Linear:** cancel or delete an issue, reassign it, change its project,
re-estimate it, edit its body, or reorder priority. These are cheap in Linear, ambiguous in
chat, and none of them is urgent. The framework deliberately ships a small vocabulary — a
verb that is guessed at is worse than a verb that does not exist.

### Binding a message to an issue

A reply can only control an issue the system can identify. Binding is recorded **in
Linear, never on the surface**: every outbound message about an issue posts a comment on
that issue carrying the surface message id. That is the whole map, it is versioned where
the work is, and it survives every session restart.

An inbound message binds if it is a reply to a bound outbound message, or if it names an
issue identifier. Otherwise it is capture.

### Stale replies

The Commander may reply hours later, to a message whose issue has since moved.

1. **Read the issue's current state first.** Always. Never transition from the state the
   outbound message remembered.
2. If the current state still satisfies the verb's precondition → apply it.
3. If the verb's target is already the current state → silent no-op plus an ack.
4. **If the state has moved such that the verb no longer applies → do not transition.**
   Reply with the issue's current state and re-offer the verbs that now apply. Never
   guess what the Commander would have wanted about the newer state.

### Verified writes

`save_issue` **silently drops writes under concurrency** — it returns a full,
success-shaped response while the issue keeps its old value; the only tell is an unchanged
`updatedAt` (`ops/capability/misses.md`, 2026-09-08).

Therefore: **a write is not done until it has been read back.** Every transition in this
file is write → fresh read → compare. Retry up to three times, then stop and tell the
Commander the transition failed and must be made in Linear. Never report a state from a
write response. Batch bulk writes at ≤10 concurrent and verify with a full sweep read.

### Stakes → volume

The old stakes table governed *authority*. It now governs only *how loudly* an item is
surfaced. Nothing here lets anything proceed unasked.

| Stakes | How it surfaces | Examples |
|---|---|---|
| Routine | Digest line only | merge a clean PR, publish a draft, dispatch a sub-agent |
| Material | Digest line, flagged first in its section | ship to prod, send a partner email, change a price |
| Strategic | Its own out-of-cycle message | sign a contract, deprecate a project, change OKRs |

### Staleness — silence parks, it never ships

Items must not accumulate in the decision queue forever, and the escape valve must run in
the safe direction.

- An issue sitting in `Triage` for **14 days** with no Commander touch moves to
  `Backlog` with a comment saying why. It has not been rejected and it has not been
  approved; it has stopped competing for attention.
- The digest reports this as one line ("Parked N stale items"), never as N lines.
- This is not consent-by-silence. Silence de-escalated the item. Nothing was dispatched,
  nothing shipped, and the issue is still there.
