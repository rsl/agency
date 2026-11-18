---
id: handoff
description: Use when the user says "handoff", "make a handoff", "handoff doc", "update handoff", "wrap up", or when a session is ending or pausing — write or update the branch's handoff, or seed a design handoff for a design-tool session (doop and friends).
---

# Handoff — writing (close-out)

Ephemeral session state lives in the working project's `.agent-notes/` — the public tree never contains it. One live handoff per in-flight branch; a branch is never created just to have a handoff slot. One *design* handoff per off-context design session, described at the end.

## The close-out (session ending or pausing, any state)

1. Promote open items AND decisions to their durable homes. "Todo" in the handoff is not promotion — the registry entry must exist. Decisions promote too: they hide once in code, nothing screams for them. An unpromoted open item or decision is a bug.
2. **Routing check** — before writing, ask: does this session's work belong to the working project? If the work spans repos or belongs to none (machine-level setup, harness work), use the question tool: route the handoff to the primary repo involved (the session can move), keep it here, or skip the handoff entirely — the right answer when the work is done and its durable state lives elsewhere; promote any open items and write nothing. Never let "the cwd had an `.agent-notes/`" decide by default.
3. Write or overwrite `.agent-notes/handoff-<branch>.md` for the branch this session worked on (`main` counts; a `/` in the name becomes `-`). Every close-out writes it, clean boundary or not — a clean-boundary handoff is short: state sentence, PR pointer, registry pointer. Mid-flight work and unpromoted judgment also earn one registry open-item naming the branch.
4. Rename this session to a short accurate title of the work (max 6 words) via `tools.opencode.session_rename` in Code Mode — titles are how open sessions are found later. If the tool is unavailable, say so and suggest the manual fallback: `/retitle`, then paste the result into ctrl+r.
5. If this session witnessed another branch merge or die, move that branch's handoff into `sessions/` — annotate what the merge proved, cite the work's shas. Never archive on speculation, and never archive your own.

## Write-only, always

The close-out is write-only work — not just at the context limit. No finishing the current task first, no edits, no fixes, no specs, no quality passes, no fluffing — anything found mid-close-out becomes a registry open item, not an action. The handoff exists so a fresh session can take over: write it, rename the session, stop.

## Trigger conditions — when to write or update the handoff

- The user signals session end ("wrap up", "that's it for today", "update the handoff") — write it immediately, don't finish the current task first.
- Open items exceed what fits in working memory, or the session will pause across a risky boundary (migration, deploy, long-running job).
- The user goes quiet is NOT a trigger you can wait for — if work is mid-flight, write the handoff BEFORE stopping.
- Maintain the handoff as you go — writing from memory at the end of a long session is how handoffs become fiction.

## Splitting work across sessions

- Punt freely, at any stable point (ideally before commit): code review, quality runs, spec audits, security scans — crisp artifacts (diff in, findings list out) survive the handoff.
- Never split editing, or unplanned refactoring, mid-feature — every boundary across the plan→code interface turns falsified assumptions into round trips.
- Planning and editing stay in one session. The plan doc records rejected alternatives and judgment calls — the next session recovers facts, not judgment.

## Handoff doc template

Dense narrative over bullet fragments. Lead with the state sentence: what shipped, what's open, where the tree stands (branch, suite counts). Sections that earn their place:

- **Built** — what was made, with design decisions and their WHY, the alternatives rejected, and the scope judgment calls.
- **The open problem** (if any) — symptoms, what's ruled out, the next diagnostic, repro command. Enough that a fresh context can continue the hunt without re-deriving.
- **Planned, not built** — designs agreed in discussion, pointing at their plan docs.
- **The developer's review checklist** — concrete clicks/actions to verify.
- **Open items → registries** — one line confirming every open item's registry location, plus any meta-decisions pending.

Repro commands go in the doc verbatim (tmp/ scratch paths included). Honesty over polish: "verified" means it was run; "should work" is flagged as such.

## Commits never appear

A handoff never mentions commits — not the commit state of the work ("uncommitted pile", "ready to commit"), not a pending commit decision ("commit call is the developer's"), not a suggestion that anything be committed. Committing is the developer's own action, taken on their explicit word, invisible to the handoff and the registry's pending work. Describe the tree by branch and suite counts; record work as what was built, which files it touched, and how it was verified. This law also covers the close-out conversation: no prompting the developer to commit, ever.

## Project parameters (the two lines each project must carry)

The protocol only fires if the project's always-loaded instructions point at it. Each project using handoffs carries, in its AGENTS.md or .agent-notes/README.md:

1. A pointer line: "Session handoffs follow the handoff skill; state lives in .agent-notes/handoff-<branch>.md — one per in-flight branch."
2. Its parameters: the **open-items registry path**, and the **notes tracking policy** (what in .agent-notes/ is tracked vs gitignored).

## Design handoffs (a second kind, not a branch handoff)

When a design-tool session (doop canvas, or any off-context design agent) will explore UI before code exists, seed it with `.agent-notes/handoff-<tool>-<topic>.md` (e.g. `handoff-doop-run-ui.md`) — keyed to the tool and purpose, NOT the branch, and never a second source of truth for build state. Write it BEFORE the session starts, not after: a fresh design session inherits nothing from this conversation, and a brief written after the fact reverse-engineers decisions from frames while the uncaptured ones get silently re-guessed.

It carries intent, not state — a spec, not a close-out:

- The agreed decisions, as constraints (layout, behavior, what's settled and why in one clause each).
- Only the surface the design agent needs — for UI work, what the relevant views/partials render; none of the code anatomy behind them.
- What's OPEN for the design agent to explore — that is what the tool is for.
- Build-context one-liners so the design translates (styling system, view conventions, dynamic-wiring mechanism).

Two cadence rules: maintain it during the design conversation (same rule as branch handoffs — end-of-session writing becomes fiction), and the design session itself appends its outcome at close-out (canvas URL, canonical frames/ids) so the eventual build session reads ONE file and gets decisions plus visual spec together.