# agency

agency is the shareable half of RSL's agent setup: rules, agent definitions, commands, and skills that coding-agent harnesses can load, whatever the harness. It's opinionated, on purpose — about where files live, what belongs to a harness versus a person, and what never gets committed. The other half is personal by design: machine-local rules, personal skills, anything describing one person's infrastructure. That stays in an untracked layer, never committed here.

## Requirements

- Ruby 4.0+ via any version manager (chruby, rbenv, mise, asdf) — needed by `agency notes` only; macOS system Ruby 2.6 won't parse it.
- `~/.local/bin` on PATH — `bin/agency install` links `agency` there.
- zsh — `bin/agency` is a zsh script (macOS default shell).

## Layout

- `AGENTS.md` — shared rules: communication style, context economy, gotchas. Intended to load before a machine-local rules file, which carries the personal details this file must not.
- `agents/` — agent definitions (mode, permissions, model) such as a pair-programmer and a read-only code reviewer.
- `commands/` — slash commands, e.g. a drift command that re-reads the agent's definition from disk and re-confirms its rules.
- `skills/` — skills in the `SKILL.md` convention: `git`, `specs`, `kickoff`, `handoff`, `red`, and others.
- `skills/personal/` — gitignored. Machine-local skills (home automation, personal task systems, box-specific runbooks) live here in your own checkout; the harness discovers them like any other skill because the directory acts as a grouping level, while git keeps them out of the repo.
- `bin/agency` — install, status, and the notes tooling. `install` creates missing symlinks from the generic directories into each harness's discovery locations, plus the `agency` link (see Requirements); `status` reports every mapping as linked, drifted, conflict, or missing. Idempotent — it never clobbers an existing real file. OpenCode discovers `skills/` natively, so only `agents/` and `commands/` need links.
- `bin/notes` — the Ruby implementation behind `agency notes <cmd>`. Report, stale, lint, format over a project's `.agent-notes/`; run from the project root whose `.agent-notes/` you mean.

## Install

Clone this repo anywhere, link it as `~/.agents` (the path most harnesses discover automatically), and let `bin/agency install` wire up your harness:

```sh
git clone <repo-url>
ln -s "$PWD/agency" ~/.agents
cd agency
bin/agency install opencode   # or whichever harness you use
bin/agency status opencode
```

## Using it

`agency notes report` gives a TOC over a project's `.agent-notes/` (also: stale, lint, format); `agency notes expire` lists sessions/ docs older than 30 days, and `agency notes expire -- FILE` deletes exactly the named sessions/ files.

The machine-local rules file and `skills/personal/` entries are optional — a fresh clone with neither works; they exist so you can add rules and skills you don't want in the shared tree. Some agents carry deny rules for MCP servers used in the original setup (for example `doop`, `things`); in a setup without those servers the rules are dead entries and nothing depends on them. Keep anything identifying, secret-bearing, or single-machine out of the shared tree.

## License

AGPL-3.0. See `LICENSE`.
