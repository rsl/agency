---
id: agent-notes
description: Use when creating or editing files in .agent-notes/.
---

# Agent notes

Every markdown note in the working project's `.agent-notes/` carries YAML
frontmatter — except `handoff.md`, `sessions/`, and `personal/`, which are
exempt (ephemeral layer). Category dirs (`plans/`, `research/`) are
durable and carry frontmatter too.

## Fields

- `title` — optional; falls back to the H1 or filename.
- `status` — required: `active`, `parked`, or `done`; update it as the
  note's state changes.
- `summary` — required, one line: the agent-facing triage line. How an
  opening act or weekly review decides which note to open. Never displayed
  by `agency notes report` — it lives in the file.

## Naming

- Point-in-time notes (`sessions/`, `personal/`) are `YYYY-MM-DD-kebab-slug.md`,
  named for the day they were written.
- Root notes are living documents and stay undated — strip the date when
  promoting a note to root.
- Category dirs are the writer's home, the permission boundary made
  physical: `plans/` (planning agent's intent docs, topic slugs like
  `plans/orchestration.md`) and `research/` (research agent's
  understanding docs). Each category agent writes only its own dir; root
  notes are duet/harness-written; the ephemeral layer is denied to
  category agents.

## Sensitivity

Customer names, project names, and incident-specific production details
belong in access-controlled systems (tracker, incident tool) — reference
the tracker instead ("affected projects listed on <card link>"), never in
`.agent-notes/`. Applies to every note type: `personal/` is exempt only
because it's gitignored — anything tracked is source history.

## Tooling (`agency notes`, on PATH after `bin/agency install`)

Run from the project root whose `.agent-notes/` you mean. Requires Ruby
4.0+ via any version manager — macOS system Ruby (2.6) won't run it.

- `agency notes report` — grouped TOC by status (durable layer: root +
  `plans/` + `research/`).
- `agency notes lint` — exits non-zero on missing or invalid frontmatter.
  Durable notes only; the ephemeral layer is exempt. Wires into hooks.
- `agency notes format` — autofills missing fields; review the diff, the
  derived summary is a starting point.
- `agency notes stale [days]` — durable notes untouched for N days (default
  90); `personal/` listed separately.
- `agency notes expire [days]` — lists `sessions/` docs older than N days
  (default 30) with date and summary. Nothing deletes without naming files:
  `agency notes expire -- <file> ...` removes exactly those paths, refusing
  anything outside `sessions/`. Agents relay the listing (e.g. as question
  options); the user decides what dies.
- `sessions/` decay is `expire`'s job, not `stale`'s.