---
id: handoff
description: Use when a session is ending or pausing, when writing or consuming a handoff doc, or when the user asks for a wrap-up or restart summary.
---

# Handoff

Ephemeral session state lives in the working project's `.agent-notes/` —
the public tree never contains it.

## The layout

- `.agent-notes/handoff.md` — fixed path, gitignored, overwritten at every
  close-out. Always the current session state.
- `.agent-notes/sessions/` — dated graveyard (`YYYY-MM-DD-<slug>.md`, named
  by birth date), gitignored, ~30-day expiry via `agency notes expire`.
- `.agent-notes/personal/` — scratch, gitignored.
- Root notes — tracked living docs, the durable layer, frontmatter contract
  per the agent-notes skill.

## The lifecycle — two acts

**Close-out** (session ending or pausing):

1. Promote open items AND decisions to their durable homes. "Todo" in the
   handoff is not promotion — the registry entry must exist. Decisions hide
   (they feel resolved once in the code; open items scream) — they promote
   too. An unpromoted open item or decision is a bug.
2. Copy the outgoing `handoff.md` into `sessions/` under its birth date.
3. Overwrite `handoff.md` with current state. One copy, one write.

**Opening** (session starting):

1. Read `handoff.md` if it exists.
2. Verify its durable claims are actually durably recorded — anything still
   living only in the handoff gets promoted now (fix forward), then work.
3. Read the project's registry and resume where the handoff points.

## Trigger conditions — when to write or update the handoff

- The user signals session end ("wrap up", "that's it for today", "update
  the handoff") — write it immediately, don't finish the current task first.
- Open items exceed what fits in working memory, or the session will pause
  across a risky boundary (migration, deploy, long-running job).
- The user goes quiet is NOT a trigger you can wait for — if work is
  mid-flight and uncommitted, write the handoff BEFORE stopping.
- Maintain the handoff as you go. Writing it from memory at the end of a
  long session is how handoffs become fiction.

## Splitting work across sessions

- Punt freely, at any stable point (ideally before commit): code review,
  quality runs, spec audits, security scans. Crisp artifacts — diff in,
  findings list out — survive the handoff.
- Never split editing, or unplanned refactoring, mid-feature: the plan→code
  interface is a falsification loop, and every boundary across it turns
  falsified assumptions into round trips.
- Planning and editing stay in one session. The plan doc records the
  rejected alternatives and the judgment calls — a doc recovers facts, not
  judgment; without them the next session re-litigates.

## Handoff doc template

Dense narrative over bullet fragments. Lead with the state sentence: what
shipped, what's open, where the tree stands (shas, suite counts,
uncommitted?). Sections that earn their place:

- **Built** — what was made, with design decisions and their WHY, the
  alternatives rejected, and the scope judgment calls.
- **The open problem** (if any) — symptoms, what's ruled out, the next
  diagnostic, repro command. Enough that a fresh context can continue the
  hunt without re-deriving.
- **Planned, not built** — designs agreed in discussion, pointing at their
  plan docs.
- **The user's review checklist** — concrete clicks/actions to verify.
- **Open items → registries** — one line confirming every open item's
  registry location, plus any meta-decisions pending.

Repro commands go in the doc verbatim (tmp/ scratch paths included).
Honesty over polish: "verified" means it was run; "should work" is flagged
as such.

## Project parameters (the two lines each project must carry)

The protocol only fires if the project's always-loaded instructions point
at it. Each project using handoffs carries, in its AGENTS.md or
.agent-notes/README.md:

1. A pointer line: "Session handoffs follow the handoff skill; state lives
   in .agent-notes/handoff.md."
2. Its parameters: the **open-items registry path**, and the **notes
   tracking policy** (what in .agent-notes/ is tracked vs gitignored).