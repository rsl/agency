---
id: handoff-read
description: Use when the user says "read handoff", "read the handoff", "pick up on the handoff", or when a session starts and should consume the current branch's handoff.
---

# Handoff — reading (opening act)

Ephemeral session state lives in the working project's `.agent-notes/` — the public tree never contains it. One live handoff per in-flight branch, keyed by branch name; the opening act knows the key from git. Do not collapse back to a single file without new information, and do not read the per-branch design as an argument for creating branches — a branch is never created just to have a handoff slot.

## The layout

- `.agent-notes/handoff-<branch>.md` — the live handoff for that branch (gitignored). One file per in-flight branch. `main` counts as a branch. A branch name containing `/` keeps the name with `/` replaced by `-`.
- `.agent-notes/sessions/` — the graveyard (`YYYY-MM-DD-<slug>.md`, gitignored). Dead handoffs only: moved here when their branch merges or dies, never as a routing step.
- `.agent-notes/personal/` — scratch, gitignored.
- Root notes — tracked living docs, frontmatter contract per the agent-notes skill.

Legacy: a project may still carry a `handoff.md` from the single-file protocol. On first touch, `mv` it to `handoff-<branch>.md` for the branch it describes.

## The opening act (session starting)

1. `git branch --show-current`, then read `.agent-notes/handoff-<branch>.md` if it exists. No file means fresh territory for this branch — or a clean-boundary branch whose state lives in its PR and the registry. A null result is an answer, not a hunt — and only a positive probe makes it one; when challenged, re-probe, never defend a null with a mechanism story. Never widen the search: no directory sweeps, no other repos' `.agent-notes/`. A topic request ("the handoff about X") is legitimate and may land on any branch: one listing, match topic to branch from filenames or the registry, read exactly that file. If two files plausibly match, ask which one — never read the nearest-looking name and call it a match.
2. Verify the handoff's durable claims are actually durably recorded — anything still living only in the handoff gets promoted now (fix forward), then work. An unpromoted open item or decision is a bug.
3. Read the project's registry and resume where the handoff points.
4. `ls .agent-notes/handoff-*.md` — the other files are other branches in flight. Filenames only — lifecycle awareness, not an invitation to read; the only handoff file a session reads unasked is its own branch's. Leave them alone: don't read them into this branch's work, don't archive them, don't merge their concerns. If one's branch no longer exists (merged, deleted), archiving it is yours to do.