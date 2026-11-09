---
status: active
summary: Consolidated empirical findings on opencode's permission chain — frontmatter layering, last-match, policies, hidden agents, category-dir boundaries. Written live to verify the research/ carve-out.
---

# opencode permission mechanics — consolidated empirical findings

These facts were established across three sessions (overnight design
tests Nov 5, fork-2 planning-agent work Nov 6, the research/ re-scope
Nov 6) and kept being re-derived from scattered notes. This note is the
standing reference. Written by the research agent as its first live write
into `.agent-notes/research/` — the write itself verified the carve-out
this note describes.

## The chain and its semantics

- Agent-file frontmatter `permissions:` (V2 plural array) is honored and
  lands LAST in the chain — after global config rules. The Oct 2026
  loader quirk (only the singular object merged) is fixed.
- Under last-match semantics, later rules win: frontmatter rules override
  same-resource global rules. The old config comment "denies cannot be
  overridden by allows" was wrong.
- Consequence for broad-allow agents (yolo): a frontmatter `allow *` first
  kills every global ask, then dangerous-class denies appended after it
  match later and win. Order within frontmatter is load-bearing.
- Top-level `policies` (schema shape `{"action": "tool.use",
  "resource": "shell:<pattern>"}`) hard-deny after everything and are
  un-overridable from any agent file under either semantics. The
  catastrophic set (sudo, git push, rm -rf, pipes to sh, ...) lives
  there. Empirically confirmed: `sudo echo hi` under yolo's allow-everything
  → "Blocked by configuration policy". Unverified empirically: policy deny
  vs frontmatter allow (expected per docs; check on yolo's first night).
- Config `agents.<name>.permissions` REPLACES a built-in agent's edit
  rules rather than appending (proven via the plan-mode block), but does
  NOT override a same-named file agent's frontmatter — config can only
  add rules a file agent doesn't already rule on.

## Tools and visibility

- A fully-denied tool is REMOVED from the tool catalog entirely (same
  mechanism as MCP instruction-block gates) — the model cannot even
  attempt it. Verified: yolo's toolset has no question tool.
- `hidden: true` in the `agents.<name>` config block removes an agent
  from the TUI picker/tab cycle; the agent stays defined (built-in
  `plan` uses this).

## The boundary pattern (category-dirs-by-writer)

Permission boundaries align with directories exactly; filename prefixes
are approximate because V2 globs' `*` crosses `/`. The durable layer:

- root notes — duet/harness-written
- `plans/` — planning agent (`edit *` deny → `.agent-notes/plans/*` allow)
- `research/` — research agent (same shape, this note's home)
- `sessions/`, `personal/`, `handoff.md` — ephemeral, denied to category
  agents.

Frontmatter rules land last, so the narrow allow beats the broad deny
under last-match. Verified live Nov 6: this write succeeded, and a
deliberate probe outside `research/` was denied (see session report).

## Saved permission approvals (the DB layer)

opencode's "always" reply on a permission ask writes a PERMANENT row to
`~/.local/share/opencode/opencode.db` (`permission` table, keyed by
project), layered into every future session before the ask fires. Saved allows override config asks indefinitely; config *denies*
still win (cannot be overridden by any allow). "Always" is a deliberate,
permanent, per-project grant.