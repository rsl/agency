# Shared agent guidance

Portable instructions that apply across projects and tools. Keep this free of
personal details and machine-specific paths — this file ships in the public
`agency` repo.

Personal rules live in a separate machine-local file (never committed) and load before this one.

## Developer & machine

- Simplest thing that could possibly work — exception: public interfaces and data contracts get upfront design.
- No trailing comments in any language — comments go above the code they describe.
- State this session didn't cause or expect (files changed, keys exist, values off): ask "did you edit this?" before hunting for provenance.
- Report scan findings, don't fix unasked — including gate failures and findings in code written this session. Report with options and a recommendation; fix only on explicit approval.

## Communication

- Lead with the answer; no warm-up, no closers, no filler.
- On failure: one brief acknowledgment.
- No emojis anywhere.
- Prose first; no section headers unless the answer needs them.
- Markdown: one paragraph, bullet, or sentence per line — let editors soft-wrap.
- Skill frontmatter: `description` unquoted when possible, single quotes otherwise.
- Match the register to the reader — casual with the user; for anyone else, whatever fits them.

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
- Scratch files go in the project's `tmp/`, never a system temp dir.
- File changes go through `edit`/`write` only — shell one-liners mangle formatting. Target exact anchors over rewrites; after two failed edits, re-read and widen.
- Shell read commands: one command per call, batched as parallel tool calls. Drop command substitutions `$(...)` and fd redirects like `2>/dev/null` from read commands, and let stderr show rather than suppressing it.
- Some shells (notably zsh, the default on macOS) abort the whole line on a failed glob ("no matches found") — later commands never run, so "not found" may mean the search never ran. List files explicitly or guard globs.

## Notes and handoffs (this repo)

Session handoffs follow the handoff skill; state lives in
`.agent-notes/handoff.md` (gitignored, overwritten at close-out). Open-items
registry: `.agent-notes/open-items.md`. Notes tracking:
root `.agent-notes/` docs are tracked; `handoff.md`, `sessions/`, and
`personal/` are gitignored.
