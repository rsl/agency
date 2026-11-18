---
id: handoff-read
description: Use when the user says "read handoff", "read the handoff", "pick up on the handoff", or when a session starts and should consume the current branch's handoff.
---

# Handoff — reading (opening act)

Ephemeral session state lives in the working project's `.agent-notes/` — the public tree never contains it. One live handoff per in-flight branch, keyed by branch name; design-tool sessions carry their own `handoff-<tool>-<topic>.md` briefs (see the handoff skill), read only when that session asks for them by name. Do not collapse back to a single file without new information.

## The layout

- `.agent-notes/handoff-<branch>.md` — the live handoff for that branch (gitignored). One file per in-flight branch. `main` counts as a branch. A branch name containing `/` keeps the name with `/` replaced by `-`.
- `.agent-notes/handoff-<tool>-<topic>.md` — design-session briefs (e.g. `handoff-doop-run-ui.md`, gitignored). Not branch handoffs: intent for an off-context design session, not build state. Their close-out outcome (canvas URL, canonical frames) lands back in the same file; the build session reads that one file for decisions plus visual spec.
- `.agent-notes/sessions/` — the graveyard (`YYYY-MM-DD-<slug>.md`, gitignored). Dead handoffs only: moved here when their branch merges or dies, never as a routing step.
- `.agent-notes/personal/` — scratch, gitignored.
- Root notes — tracked living docs, frontmatter contract per the agent-notes skill.

Legacy: a project may still carry a `handoff.md` from the single-file protocol. On first touch, `mv` it to `handoff-<branch>.md` for the branch it describes.

## The opening act (session starting)

1. `git branch --show-current`, then read `.agent-notes/handoff-<branch>.md` if it exists. No file means fresh territory for this branch — or a clean-boundary branch whose state lives in its PR and the registry. A null result is an answer, not a hunt — and only a positive probe makes it one; when challenged, re-probe, never defend a null with a mechanism story. Never widen the search: no directory sweeps, no other repos' `.agent-notes/`. A topic request ("the handoff about X") is legitimate and may land on any branch: one listing, match topic to branch from filenames or the registry, read exactly that file. If two files plausibly match, ask which one — never read the nearest-looking name and call it a match.
2. Verify the handoff's durable claims are actually durably recorded — anything still living only in the handoff gets promoted now (fix forward), then work. An unpromoted open item or decision is a bug.
3. Read the project's registry and resume where the handoff points.
4. `ls .agent-notes/handoff-*.md` — the other files are other branches and design briefs in flight. Filenames only — lifecycle awareness, not an invitation to read; the only handoff file a session reads unasked is its own branch's, and a design brief only when its session asks for it by name. Leave them alone: don't read them into this branch's work, don't archive them, don't merge their concerns. If one's branch no longer exists (merged, deleted), archiving it is yours to do.
5. Rename this session to the project and branch (`<project> <branch>`; project is the repo directory's basename) via `tools.opencode.session_rename` — open sessions are told apart by project and branch, and a bare or topic-only title hides both. If the tool is unavailable, say so and suggest `/retitle`.