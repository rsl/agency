# agency

Portable agent setup: shared rules, agent definitions, commands, and skills
that coding-agent harnesses can load. The repo is deliberately
harness-agnostic and user-agnostic — machine-local rules, personal skills,
and anything describing one person's infrastructure live in a separate
untracked layer, never committed here.

## Requirements

- Ruby 4.0+ via any version manager (chruby, rbenv, mise, asdf) — needed by
  `agency notes` only; macOS system Ruby 2.6 won't parse it.
- `~/.local/bin` on PATH — `bin/agency install` links `agency` there.
- zsh — `bin/agency` is a zsh script (macOS default shell).

## Layout

- `AGENTS.md` — shared rules: communication style, context economy, gotchas.
  Intended to load before a machine-local rules file, which carries the
  personal details this file must not.
- `agents/` — agent definitions (mode, permissions, model) such as a
  pair-programmer and a read-only code reviewer.
- `commands/` — slash commands, e.g. a drift command that re-reads the
  agent's definition from disk and re-confirms its rules.
- `skills/` — skills in the `SKILL.md` convention: `git`, `specs`,
  `kickoff`, `handoff`, `red`, and others.
- `skills/personal/` — gitignored. Machine-local skills (home automation,
  personal task systems, box-specific runbooks) live here in your own
  checkout; the harness discovers them like any other skill because the
  directory acts as a grouping level, while git keeps them out of the repo.
- `bin/agency` — install, status, and the notes tooling. `install` creates
  missing symlinks from the generic directories into each harness's
  discovery locations, plus the `agency` link (see Requirements); `status`
  reports every mapping as linked, drifted, conflict, or missing. Idempotent
  — it never clobbers an existing real file. OpenCode discovers `skills/`
  natively, so only `agents/` and `commands/` need links.
- `bin/notes` — the Ruby implementation behind `agency notes <cmd>`. Report,
  stale, lint, format over a project's `.agent-notes/`; run from the project
  root whose `.agent-notes/` you mean.

## Using it

Point your harness at this checkout (most discover `~/.agents/skills` and
`AGENTS.md` automatically), then run:

```sh
bin/agency install opencode   # or whichever harness you use
bin/agency status opencode
agency notes report           # TOC over .agent-notes/ (also: stale, lint, format)
agency notes expire           # list sessions/ docs older than 30 days
agency notes expire -- FILE   # delete exactly the named sessions/ files
```

Add your own machine-local rules file and `skills/personal/` entries; keep
anything identifying, secret-bearing, or single-machine out of the shared
tree.

## License

AGPL-3.0. See `LICENSE`.