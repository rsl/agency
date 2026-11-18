---
description: Use when a check runner's findings need walking — a proactive quality or review pass, or a full findings list to report and fix. A single failing check mid-run is the red skill's territory instead.
---

A findings list is walked, never solved in one shot. The failure mode to defeat: an agent runs the check, sees 40 offenses, and "helpfully" fixes all of them in one pass — unreviewed, unapproved, wrong.

1. **Run the check, do not act.** Use the project's own check runner — `bin/quality` when it exists, otherwise whatever AGENTS.md names as the gate runner — never the bare linters: the runner carries the project's config, and bare invocations report wrong findings. Report totals grouped by tool: autocorrectable vs manual.

2. **Propose a working order and wait for agreement.** Default:
   - Safe autocorrectables
   - Unsafe autocorrectables, small reviewed groups
   - Judgment-call offenses (security, naming, config)
   - Metrics refactors — real design decisions, one method at a time

   The user may prefer offense-by-offense instead of batches — ask. Agreement on the working order authorizes walking the groups in that order — it is not approval of any fix. Autocorrectable describes the diff, not the consent: a "safe" fix still needs its own yes, and if the user's reply could be read as approving either the order or the fixes, treat it as approving the order only. Each group still gets its own shown before/after and its own yes. The two hard gates — working order, and apply-or-not per group — always go through the `question` tool; if the environment has none, lettered options in chat stand in, never a freeform ask. In an unattended (yolo) session the gates are unanswerable — do not walk, do not self-approve: run the check read-only if the session's contract calls for it, record the grouped totals in the handoff, and end the session — the handoff names the review walk as the next session's opening act. The commit-time hook is the enforcement either way. Resuming with findings already on screen skips the run-up — it happens once per session.

3. **One offense group at a time.** For each:
   - The fix for an offense is code, not a suppressor: never reach for a disable comment; if one is genuinely warranted it is line-scoped (`# rubocop:disable-next <Cop>` — `disable-next-line` does not exist in RuboCop and writes an unrecognized directive) with a why-comment, and that is itself a proposal.
   - Show the site(s) and a concrete before/after proposal. When a fix requires anything beyond the minimal mechanical change (renames, new copy, extracted helpers), name those in the proposal — behavior changes are called out explicitly, never smuggled into a "stylistic" fix.
   - Discuss — the user may refactor, suggest alternatives, or defer to a "won't fix" bucket.
   - **Apply only on explicit approval**, in small chunks — one group per pass, a minimal diff the user can review inline; never batch groups into one pass. End the turn after showing each group's before/after: applying while the conversation is still open — because the approval hasn't had a chance to arrive — is how batch-apply happens. If the applied change balloons beyond what was shown, stop and re-propose. Re-run the check on the affected files and confirm the offense cleared.
   - Refactors that clear one offense often introduce new ones (complexity shifts shape elsewhere and re-flags). Stop and report each new finding with options before fixing it — it counts as a new group under this step, not part of the approved fix.

4. **Guardrails:**
   - Never tune the lint/gate config to make a finding disappear; a config change is a proposal like any other, and some repos keep their lint config in a separate synced source — check before proposing edits to it.
   - Dead code: confirm zero callers before proposing deletion; deletion still needs approval.
   - Keep a running "won't fix" bucket; surface it at the end for a final decision (inline disable vs config change vs leave as-is).

If a per-repo companion skill exists for this environment, load it before walking — it carries the tool specifics this skill generalizes: ERB/herb grammar, cop names, where lint config lives. Per-repo skills add only specifics on top of this flow and never redefine the walking itself.