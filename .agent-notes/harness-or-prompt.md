---
title: Harness vs prompt vs model — what makes agents collaborative
status: active
summary: Nov 5 session findings — harness is exonerated for rsl's GLM setup (verified in opencode source); anti-collaboration lives in per-model prompt templates and model prior. Research ladder for enforcement hooks, prompt porting, and effort A/B.
---

# Harness vs prompt vs model — collaboration behavior

Question that started it (Nov 5, langafile session): would building rsl's own
agent harness improve collaborative behavior — agent ignoring instructions,
starting work without collaboration?

## Findings from the Nov 5 session

- **Controlled experiment already ran.** Two harnesses (opencode, pi) + same
  agent files (AGENTS.md, ~/agents/) = same defiance. Harness eliminated as
  the variable.
- **pi was worse on its own merits.** Its minimal "build it yourself" prompt
  loses to opencode's short-but-tuned base. Verdict: the harness philosophy
  diff is *actually* a system-prompt diff. Not a build — a maintained system
  prompt, which is exactly the maintenance cost building your own harness
  takes on.
- **opencode 2.2.5 source read** (cloned to
  `/private/var/folders/4h/.../opencode/opencode-repo`, HEAD 97178e5):
  - Base harness prompt (`packages/core/src/session/runner/prompt/system.txt`)
    is ~150 tokens, neutral, even a protective clause against overwriting
    unknown work. No proaction bias.
  - Per-model templates override it in
    `packages/core/src/plugin/system-prompt/*.txt`, selected by id match in
    `optimize.ts`: `gpt`/`astra` → gpt.txt, `kimi` → kimi.txt,
    `trinity` → trinity.txt, `muse` → meta.txt. Offenders:
    - gpt/kimi templates: "default to taking action with tools" +
      "When the request could be interpreted as either a question or a task,
      treat it as a task" — the single worst anti-collab sentence.
    - meta.txt: "proactively use subagent to launch specialized subagents".
    - trinity.txt: Claude-Code-style `# Proactiveness` section.
  - **GLM ids match none of these.** On opencode, GLM gets the neutral base
    prompt. So for rsl's GLM-on-omlx setup, the proaction the model shows is
    the model's own agentic training prior, uncounterweighted.
- **Context-stack breakdown** (from the session): ~10k tokens of system
  context; by influence on collaboration answers, the duet preamble + agency
  conventions carry ~90%, project AGENTS.md dominates only for work tasks,
  harness baseline ~nothing.

## Open research items

1. **omlx proxy passthrough check** — does omlx (8888/8890) splice, trim, or
   reorder system prompts in transit? If it normalizes, the duet preamble's
   position shifts and instruction-following on GLM shifts with it. Check
   middleware/logs of its request path.
2. **reasoning_effort A/B** — same duet task at low/high/max (config variant
   bodies already exist). More thinking tokens may *reduce* preamble
   compliance. Cheap to run from opencode.jsonc only.
3. **Foreseeable-violation rewrite of the duet preamble** — convert "do not
   fire-and-forget" style rules into the stop-and-recheck form ("Before your
   first edit: has a plan been approved?"). Hypothesis: measured compliance
   gain from phrasing shape, not length.
4. **Mechanical gates, not prose** — pick the 1–2 most-violated rules and
   enforce them via opencode permission config (already rich in rsl's
   opencode.jsonc) or pre-edit hooks rather than prompt clauses. In pi, the
   equivalents are its example extensions (`permission-gate.ts`,
   `protected-paths.ts`).
5. **Prompt import** — if pi stays in play, its system prompt is replaceable
   (`forceSystemPrompt` via extension, or one of the
   community-hardened variants. Maintenance cost is real; only worth it if
   the omlx/GLM check (1) or effort A/B (2) shows prompt content is the
   binding constraint, not the model prior.
6. **Failure log** — one week of logged "ignored instruction" incidents,
   classified by which rule and which layer. Decides items 3 vs 4 empirically.
7. **circle.md placement** — `~/dev/agency/.opencode/agents/circle.md` is a
   placeholder; destination is `~/dev/fa/unifractured`. The duet's
   `circleci*` deny makes CircleCI unreachable outside agency until the file
   moves into that repo's `.opencode/agents/` (or global if promoted).

## Verdict so far

Anti-collab behavior = model prior weighted by (a) per-model prompt template
(gpt/kimi family) where it fires, (b) trained-agency tilt where none fires
(GLM). Harness construction is not the fix; prompt content and hard gates
are. Start with 6 (observe), then 2 and 3 (cheap), keep 4 as the enforcement
endgame.
