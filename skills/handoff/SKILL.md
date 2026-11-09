---
id: handoff
description: Use when the user says "handoff", "make a handoff", "handoff doc", "update handoff", "wrap up", or when a session is ending or pausing — write or update the branch's handoff.
---

# Handoff — writing (close-out)

Ephemeral session state lives in the working project's `.agent-notes/` — the public tree never contains it. One live handoff per in-flight branch; a branch is never created just to have a handoff slot.

## The close-out (session ending or pausing, any state)

1. Promote open items AND decisions to their durable homes. "Todo" in the handoff is not promotion — the registry entry must exist. Decisions promote too: they hide once in code, nothing screams for them. An unpromoted open item or decision is a bug.
2. **Routing check** — before writing, ask: does this session's work belong to the working project? If the work spans repos or belongs to none (machine-level setup, harness work), use the question tool: route the handoff to the primary repo involved (the session can move), keep it here, or skip the handoff entirely — the right answer when the work is done and its durable state lives elsewhere; promote any open items and write nothing. Never let "the cwd had an `.agent-notes/`" decide by default.
3. Write or overwrite `.agent-notes/handoff-<branch>.md` for the branch this session worked on (`main` counts; a `/` in the name becomes `-`). Every close-out writes it, clean boundary or not — a clean-boundary handoff is short: state sentence, PR pointer, registry pointer. Mid-flight state (uncommitted work, unpromoted judgment) also earns one registry open-item naming the branch.
4. If this session witnessed another branch merge or die, move that branch's handoff into `sessions/` — annotate what the merge proved, cite the work's shas. Never archive on speculation, and never archive your own.

## Trigger conditions — when to write or update the handoff

- The user signals session end ("wrap up", "that's it for today", "update the handoff") — write it immediately, don't finish the current task first.
- Open items exceed what fits in working memory, or the session will pause across a risky boundary (migration, deploy, long-running job).
- The user goes quiet is NOT a trigger you can wait for — if work is mid-flight and uncommitted, write the handoff BEFORE stopping.
- Maintain the handoff as you go — writing from memory at the end of a long session is how handoffs become fiction.

## Splitting work across sessions

- Punt freely, at any stable point (ideally before commit): code review, quality runs, spec audits, security scans — crisp artifacts (diff in, findings list out) survive the handoff.
- Never split editing, or unplanned refactoring, mid-feature — every boundary across the plan→code interface turns falsified assumptions into round trips.
- Planning and editing stay in one session. The plan doc records rejected alternatives and judgment calls — the next session recovers facts, not judgment.

## Handoff doc template

Dense narrative over bullet fragments. Lead with the state sentence: what shipped, what's open, where the tree stands (branch, shas, suite counts, uncommitted?). Sections that earn their place:

- **Built** — what was made, with design decisions and their WHY, the alternatives rejected, and the scope judgment calls.
- **The open problem** (if any) — symptoms, what's ruled out, the next diagnostic, repro command. Enough that a fresh context can continue the hunt without re-deriving.
- **Planned, not built** — designs agreed in discussion, pointing at their plan docs.
- **The developer's review checklist** — concrete clicks/actions to verify.
- **Open items → registries** — one line confirming every open item's registry location, plus any meta-decisions pending.

Repro commands go in the doc verbatim (tmp/ scratch paths included). Honesty over polish: "verified" means it was run; "should work" is flagged as such.

## Project parameters (the two lines each project must carry)

The protocol only fires if the project's always-loaded instructions point at it. Each project using handoffs carries, in its AGENTS.md or .agent-notes/README.md:

1. A pointer line: "Session handoffs follow the handoff skill; state lives in .agent-notes/handoff-<branch>.md — one per in-flight branch."
2. Its parameters: the **open-items registry path**, and the **notes tracking policy** (what in .agent-notes/ is tracked vs gitignored).