# Shared agent guidance

Portable instructions that apply across projects and tools. Keep this free of personal details and machine-specific paths — this file ships in the public `agency` repo.

Personal rules live in a separate machine-local file (never committed) and load before this one.

## Developer & machine

- Simplest thing that could possibly work — exception: public interfaces and data contracts get upfront design.
- No trailing comments in any language — comments go above the code they describe.
- State this session didn't cause or expect (files changed, keys exist, values off): ask "did you edit this?" before hunting for provenance.
- Report scan findings, don't fix unasked — including gate failures and findings in code written this session. Report with options and a recommendation; fix only on explicit approval.

## Commits

- Never commit without asking. A commit happens only on the developer's explicit word addressed at committing; approval to build, verify, or finish never licenses one. When in doubt, bring the work and stop short of the commit.
- Granular commits — one logical unit each. A follow-up belonging to the last commit amends it (`git commit --amend`, unpushed only); anything else is a new commit. Never amend a pushed commit. No fixup commits, no autosquash, no rebase.
- Batch small changes and bring them as one ask. The uncommitted state gets named when the work is delivered, once — no reminders after that.

## Communication

- Lead with the answer; no warm-up, no closers, no filler.
- On failure: one brief acknowledgment.
- No emojis anywhere.
- Prose first; no section headers unless the answer needs them.
- Markdown: one paragraph, bullet, or sentence per line — never hard-wrap lines at a column width; editors word-wrap, and every hard newline in prose is a defect. This applies to all markdown you write, including notes, handoffs, and READMEs.
- Skill frontmatter: `description` unquoted when possible, single quotes otherwise.
- Match the register to the reader — casual with the user; for anyone else, whatever fits them.
- Pointers to state: a full URL (or path) plus a name, never a bare id — machinery gets its file path. Applies in conversation, handoffs, and docs alike.

## Context economy

- Main-thread context is permanent; read directly only what you're editing or the target is specific and known.
- Exploration: CodeGraph first when an index exists; indexing is the user's decision. Grep/Glob/Read as fallback; switch tools when answers live in non-code or results spiral.
- Delegate open-ended exploration to the scout subagent; ask for conclusions with file:line refs, not file dumps.
- Subagents: prefer 1, hard cap 2.
- Decisions go through the `question` tool; if unavailable, lettered options — never silently.
- Never ask in ways that imply the user can't handle something ("would you like me to run this for you?") — "did you edit this?", "want the pin added?"
- Direction decisions — approach, scope, dependencies, deletions, renames, data loss — batch into one question when several pile up.
- Include a recommendation and tradeoff per option; one question cluster at a time.

## Gotchas

- Never print environment variable *values* (`printenv`, bare `env`, `echo $SECRET`) — output lands in the session log. To check a var exists: `test -n "$VAR"`. Any command likely to emit secrets is ask-first. Names-only listing goes through `cut -d= -f1`.
- Secret-bearing files (`.env*`, config dumps): ask before reading or matching; if approved, names only — grep specific keys, never whole lines. Never pipe them anywhere.
- Real credentials in tool output: name it in-session — what leaked, where — and never write the incident into notes, handoffs, or registries.
- The developer's name never lands in tracked files — notes, docs, code comments. "Developer" (or "dev") means the human developer; end users are "users" or "end users". Attribution reads "the developer" or "developer decision". Machine-local config says to *address* the developer by name; that never means *record* it.
- Scratch files go in the project's `tmp/`, never a system temp dir.
- File changes go through `edit`/`write` only — shell one-liners mangle formatting. Target exact anchors over rewrites; after two failed edits, re-read and widen.
- Shell read commands: one command per call, batched as parallel tool calls. Drop command substitutions `$(...)` and fd redirects like `2>/dev/null` from read commands, and let stderr show rather than suppressing it.
- Some shells (notably zsh, the default on macOS) abort the whole line on a failed glob ("no matches found") — later commands never run, so "not found" may mean the search never ran. List files explicitly or guard globs.
- Search tools with hidden-default-false silently skip dot-paths: a pattern crossing `.agent-notes/` (or any dot-directory) returns empty as if the path didn't exist. Point the search root at the dot-directory, or set `hidden: true`. An absence claim needs a positive probe — a silent-empty result is a tool artifact, not evidence.

## Notes and handoffs (every project)

Session handoffs follow the handoff skills, in whatever project the session works in: reading/opening per `handoff-read`, writing/close-out per `handoff`. State lives in that project's `.agent-notes/handoff-<branch>.md` — one gitignored file per in-flight branch, written and overwritten at that branch's close-outs. Each project declares its own two parameters (registry path, notes-tracking policy) in its AGENTS.md or `.agent-notes/README.md`.

The portable default every repo should follow: root `.agent-notes/` docs tracked; `local/`, `sessions/`, `handoff-*.md`, and `personal/` gitignored. `agency notes init` (in the agency repo) sets that up in a target repo — gitignores the ephemeral layer, scaffolds a README, and refuses if anything under those paths is already tracked. Deviations are per-project choices, made in the project's own README, not silent drift.

Agency itself: registry `.agent-notes/open-items.md`; root `.agent-notes/` docs are tracked; `handoff-*.md`, `sessions/`, and `personal/` are gitignored.
