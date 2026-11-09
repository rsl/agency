---
title: Open items
status: active
summary: The project's open-items registry — promoted from session handoffs; every open item and pending meta-decision lands here durably.
---

# Open items

Promoted from session handoffs per the handoff skill. Machine-local items stay in the machine-local workflow registry — each checkout's own machine-local layer — never here: this file is tracked in a public repo and must not carry machine-specific details.

- **Phase 5 — DONE Nov 6 2026, both sides.** Agency-side: the machine-local workflow-doc restructure (vocabulary rule, lifecycle-first lineup, plan-mode entry, layering rule, overnight history compressed), ripple sweep clean. The other repos' pass landed the same day and is tracked in their own registries — agency carries none of that content and no open items for it. History: `.agent-notes/plans/orchestration.md` Phase 5.
- **Post-hoc collaboration audit** — open buildable idea, salvaged Nov 9 2026 from the deleted why-agents-edit research note: the agent can't notice its own rule decay (whoa's self-catch has never fired), but the transcript always shows the unauthorized decisions — hooks or audit passes that make the transcript review itself get collaboration without needing to fix the want.
- **yolo first live night** — low-stakes fence, verify: zero prompts all night, night-queue.md written when a would-be ask arises, clean stop with handoff, morning read coherent. TUI verified Nov 6 (visible in a fresh session's cycle). Caveat: long-lived sessions can serve a stale agent registry (the server-cache gotcha) and not show yolo in the cycle — start the evening session fresh before switching. (Nov 6 2026)
- **Resolved en route (Nov 6 2026):** the global config comment claimed "denies cannot be overridden by allows" while V2 docs say last-match wins and only policies hard-deny after everything — the ambiguity mattered because yolo's broad allow-* would beat end-of-chain denies under last-match. Resolved by moving all catastrophic denies to top-level `policies` (schema-confirmed `tool.use` / `shell:<pattern>` shape) — un-overridable either way. Unverified empirically: that a policy deny actually blocks a frontmatter allow (expected per docs; check on first night). Secret-path (.env) denies stay in the permissions array and are re-stated in yolo.md's frontmatter, so they hold under either semantics.