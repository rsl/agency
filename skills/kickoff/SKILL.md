---
name: kickoff
description: Use at the start of substantive work, or when the user says "kickoff" — ask for the plan, triage it, recommend a reasoning-effort level, and whether the work should end with a review punt.
---

# Kickoff

Before starting work:

1. If the task is not already stated, ask what the plan is — one question, not an interview. Skip this step if the user already gave the plan.
2. Classify the work:
   - Blast radius: how many files; does it touch data, public contracts, or anything irreversible?
   - Ambiguity: is the approach obvious, or are there real design choices?
   - Character: mechanical (refactor, rename, boilerplate, follow an existing pattern) vs. exploratory (architecture, unknown-cause debugging, novel design)?
3. Recommend an effort tier and present it via the `question` tool with selectable options (fall back to lettered options if unavailable):
   - `low` — mechanical, small, obvious approach.
   - `medium` — normal feature work, a few files, minor choices. The default when in doubt.
   - `high` — architecture, gnarly debugging, contract-level or irreversible changes, heavy ambiguity.
   Include a one-line justification and mark the recommended option.
4. Recommend whether the work should end with a review punt — a fresh
   review-agent session on the branch once it stabilizes, ideally before
   commit — or is too small to bother. This can be folded into the same
   `question` cluster as the tier. Punt rules: the handoff skill's
   "Splitting work across sessions" section.
5. Remind the user how to apply the tier — the effort setting itself cannot be changed from inside the conversation:
   - `/variant`: switch the current model's variant (effort level) directly.
   - `/models` (or Ctrl+X then M): pick a model, optionally with a variant suffix, e.g. `provider/model#high`. Variant names come from the model's catalog entry; an unknown suffix just errors, so try another.
   - F2 cycles recently used model/variant picks.
   - Mid-session switching keeps the prompt cache on models with per-message effort updates (recent Claude Opus / GPT-6+), so re-triaging later is cheap.
6. After they pick (or decline), proceed at whatever effort is active. Do not re-ask unless the task materially changes mid-run.