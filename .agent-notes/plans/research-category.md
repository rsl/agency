---
title: Research category dir — the research/ shape
status: done
summary: Implements the Nov 6 taxonomy decision — research agent re-scoped to .agent-notes/research/*, why-agents-edit migrates, bin/notes scans category dirs. Implemented and live-verified Nov 6, same session it was designed in.
---

# Research category dir — the research/ shape

Resolves the window parked in plans/orchestration.md's Phase 4 SUPERSEDED
note. The taxonomy decision it implements: category-dirs-by-writer — root
notes are duet/harness-written durable notes, `plans/` is planning's intent
docs, `research/` is research's understanding docs, and `sessions/`,
`personal/`, and `handoff.md` are the ephemeral layer, denied to category
agents. Written live by the planning agent mid-session (this write is the
fork-2 verification item landing).

State: research agent currently holds `.agent-notes/*` allow (the hole the
SUPERSEDED note found — it includes the ephemeral layers). bin/notes scans
root notes only — `plans/` is durable but invisible to report/stale/lint.

## Decisions and why

### The permission boundary follows the writer, not the topic

`.agent-notes/research/*` allow is the whole change. Why not topic dirs or
filename prefixes: permission boundaries align with directories exactly,
while V2 globs' `*` crosses `/` and prefixes are approximate — the same
argument that put plan docs in `plans/` (orchestration.md, fork 2
resolution). The writer axis also matches the interaction contract: each
category agent owns one directory, the shared durable layer (root) stays
harness-written, and no category agent can touch another's docs or the
ephemeral layer.

### Research writes nothing ephemeral

`research.md`'s body says point-in-time scratch goes in `sessions/` or
`personal/` — both denied under the new shape. Rather than re-open a
scratch channel, research's home becomes all-durable: every doc lands in
`research/` with frontmatter, and `agency notes stale` surfaces dead
weight for the user's deletion call (no expire mechanism for research/ — add
one only if the dir actually accumulates junk). Reason: the scratch/derive
split was solving a clutter problem that `stale` already solves at the
durable layer, and two write tiers per agent doubles the permission surface
for no gain the last month of use has shown.

### bin/notes treats category dirs as durable

`root_files` becomes the durable scan: root + `plans/` + `research/`.
Concretely: a `CATEGORY_DIRS = %w[plans research]` constant and a
`durable_files` helper merging the globs. Ripple per command:

- `report` — TOC gains category docs, path shown with directory prefix
  (`plans/orchestration.md`), still grouped by status, still minimal.
- `stale` — category docs join the main scan; `personal/` stays separate.
- `format` — applies to category docs too (they carry the same
  frontmatter contract).
- `lint` — scope decision below.
- `expire` — untouched (sessions/ only).

### Fork: lint scope

The agent-notes skill says lint covers root notes only. But the
frontmatter contract already applies to every durable note, and plans/ +
research/ are durable — root-only lint enforces the contract on half the
durable layer. Recommend widening lint to `durable_files`; low-risk since
plans/ docs already carry valid frontmatter (orchestration.md passes).
Alternative: keep root-only and the contract stays unenforced on category
dirs — no upside found.

## Phases

1. **Dir + migration** — `mkdir .agent-notes/research`; `git mv` (well:
   plain mv — zero-commit tree) `.agent-notes/why-agents-edit.md` →
   `.agent-notes/research/why-agents-edit.md`. No content change; the
   frontmatter already conforms.
2. **research.md** — frontmatter allow narrows `.agent-notes/*` →
   `.agent-notes/research/*`; body rewrite: docs land in
   `.agent-notes/research/` (durable, frontmatter, promote-don't-park),
   scratch sentence removed. Same verify-on-first-run comment pattern as
   the existing carve-out.
3. **bin/notes** — durable_files helper per the decision above; report,
   stale, format, lint per fork resolution; usage-text and comment header
   updates (the "root notes only" lines).
4. **Doc ripples** — skills/agent-notes SKILL.md: taxonomy paragraph
   (category dirs by writer, research/ added, "Planning-agent writable;
   everything else is not" rephrased), lint scope line per fork,
   tooling lines ("root notes only" → durable layer). The machine-local
   workflow doc's
   registry entry: why-agents-edit path update. orchestration.md Phase 4
   SUPERSEDED note: marked implemented by this plan. Agency README: only
   if its .agent-notes/ description names the layout (check at execution).

## Verification

- `opencode debug agents` from a fresh cwd (never a long-running server's
  cwd — load-time cache): research chain shows `edit *` deny then
  `.agent-notes/research/*` allow.
- Live boundary check: research writes to research/ succeed; a deliberate
  probe at root or plans/ is denied (the deny is the point — prove it).
- `agency notes report`, `stale`, `lint` from the agency root: research/
  and plans/ docs present, why-agents-edit listed under its new path,
  lint green.
- Machine-local workflow doc's registry path updated and grep-clean of
  the old path.

## Out of scope

- `plans/` write permissions (planning agent already scoped correctly).
- Any expire mechanism for research/ (decided above: stale surfaces, the
  user deletes).
- another repo's notes migration (Phase 5's separate pass).