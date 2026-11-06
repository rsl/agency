---
title: Open items
status: active
summary: The project's open-items registry — promoted from session handoffs; every open item and pending meta-decision lands here durably.
---

# Open items

Promoted from session handoffs per the handoff skill. Machine-local items
stay in `~/.config/opencode/WORKFLOW.md` — this file is tracked in a public
repo and must not carry machine-specific details.

- **Phase 5 — agency-side DONE Nov 6 2026** (WORKFLOW.md restructure:
  vocabulary rule, lifecycle-first lineup, plan-mode entry, layering rule,
  overnight history compressed; ripple sweep verified clean — agency tree,
  README, weekly-review carry no stale references). Remaining: **FA-side
  pass, separate session** — fa-agent-notes shrinks to delta; unifractured
  lefthook pre-push retargets to `agency notes lint` (gem-side notes-lint
  moves out entirely, no transition wrappers, per fork 1); unifractured
  AGENTS.md pointer line session-handoff → handoff; unifractured
  .agent-notes/sessions/ committed-docs migration decision (FA's call);
  step-gate gem-side relocation; unifractured .agent-notes/local/ →
  personal/ rename. Details: `.agent-notes/plans/orchestration.md`
  Phase 5 section.
- **Planning-agent live verification — RESOLVED Nov 6 2026** (live session:
  no plan reminder injected on switch, `.agent-notes/plans/` write landed
  — plans/research-category.md authored by the planning agent — hidden
  built-in `plan` absent from the TUI cycle). Details:
  `.agent-notes/plans/orchestration.md` Verification items.
- **Meta-decision: tighten-calibration rule in duet.md — DECIDED Nov 6
  2026 (RSL)**: codified as one line in duet's plan-gate section —
  imperatives approve restatement kills only, judgment calls propose-wait,
  verify against the tree before claiming done. One line by design; a
  paragraph would be the accretion bias the rule guards against.
- **Research question: `.agent-notes/research/why-agents-edit.md`** —
  open, untouched since Nov 5 (migrated to research/ Nov 6).
- **yolo first live night** — low-stakes fence, verify: zero prompts all
  night, night-queue.md written when a would-be ask arises, clean stop with
  handoff, morning read coherent. TUI verified Nov 6 (visible in a fresh
  session's cycle). Caveat: long-lived sessions can serve a stale agent
  registry (the server-cache gotcha) and not show yolo in the cycle —
  start the evening session fresh before switching. (Nov 6 2026)
- **Resolved en route (Nov 6 2026):** the global config comment claimed
  "denies cannot be overridden by allows" while V2 docs say last-match wins
  and only policies hard-deny after everything — the ambiguity mattered
  because yolo's broad allow-* would beat end-of-chain denies under
  last-match. Resolved by moving all catastrophic denies to top-level
  `policies` (schema-confirmed `tool.use` / `shell:<pattern>` shape) —
  un-overridable either way. Unverified empirically: that a policy deny
  actually blocks a frontmatter allow (expected per docs; check on first
  night). Secret-path (.env) denies stay in the permissions array and are
  re-stated in yolo.md's frontmatter, so they hold under either semantics.