---
title: Orchestration tightening plan
status: active
summary: Nov 2026 session decisions — retheme renames, git policy, two-layer notes system with fixed handoff.md, notes tooling port, agent write carve-outs; phased execution brief.
---

# Orchestration tightening — Nov 2026 plan

Decisions from the Nov 5 2026 plan-mode session ("look at the skills and
agents, think about flow"). This doc is the executor's brief: decisions WITH
their reasoning (the search, not just the destination), then phases in
execution order, then open forks and verification items. Consume it by doing
the work and promoting the durable parts into the skills —
same promote-don't-park rule as everything else. First resident of this
repo's .agent-notes/, written under the conventions it describes — and its
own write (from a plan-mode session, via the .agent-notes carve-out) was the
live test of the plan-mode permission change.

## Decisions and why

### Naming — three vocabularies, one word = one meaning

Agents = who holds the context. Skills = protocols that fire on an event;
the name says when. Commands = the user's muscle memory. "Gate" exits the
base vocabulary entirely — it's machinery vocabulary other repos use locally
repos use locally, not a base word.

The naming principle, codified after the user rejected "gate" ("gate is such an
agent word — what is it to a human?"):

> No name is protected by precedent. Every pass, every name re-earns its
> place against the current lineup — "that's what we call it" is never a
> reason. Names are human words: the word a person would use for the
> moment a skill fires, not machinery vocabulary. An agent says "gate
> failed"; a human says "it's red."

Applied to the lineup, gate-failure was the only name that failed the
principle — everything else (whoa, drift, handoff, kickoff, missing-tool,
git, specs, ruby-idioms, agent-notes) passes. The principle cleaned exactly
one name, which is evidence it's the right principle.

- `red` (was gate-failure): the human-perceived state — specs are red, the
  build is red. Pairs with whoa: whoa is what a human says to stop the
  agent; red is what a human sees when the world stops the work.
  Description: "Use when a check fails — specs, hooks, quality runs, CI."

- Full retheme over collisions-only: the flat presentation of lifecycle vs
  domain skills was the "no flow" feeling; renaming is cheap now (zero
  commits, few reference sites).
- duet, research, review keep their names. duet encodes the philosophy
  (user leads, agent executes); the machine-local workflow doc is built
  on it.
- `whoa` = command AND skill, same name. The skill's trigger IS the word.
  The command stays despite the /kickoff-wrapper-removal precedent because
  whoa is an interrupt — skill loading is decided by the drifting agent
  itself; the command is the deterministic path. Planning skills don't need
  that guarantee; stop-mechanisms do.
  **Override, Nov 5 post-Phase-1 (user-confirmed, empirical): the agent never catches
  itself** — the skill's self-trigger has no instances, this session
  included (the agent tightened a command past its judgment calls while
  discussing the rule against it), and an unauthorized decision never
  feels unauthorized from the inside — it feels like progress. whoa is
  command-only like drift:
  skills/whoa/ deleted, protocol lives in commands/whoa.md. The
  command-vs-skill reasoning above described a mechanism that never fires.
- `drift` = merged re-anchor + remind-gate command, command-only, no skill.
  One action (re-read agent file, re-confirm rules), they were
  near-identical. Frees "drift" from colliding with the panic word.
- session-handoff → `handoff`; check-in → dissolved into whoa;
  missing-software → `missing-tool`; commits → `git` (widened);
  gate-failure → `red` (see naming principle below). step-gate
  relocates to another repo (references a project skill; global command calling
  project content breaks layering).
- Two base skills split by trigger moment: `handoff` (session ending,
  pausing, starting — the flow) and `agent-notes` (creating/editing
  .agent-notes files — the format). One skill with both triggers would have
  a dishonest description.

### Git policy — the publication boundary

Discovery: `git push*` was `ask` in global config, so agents could attempt
push and another repo's pre-push lefthook chain (notes-lint →
duplicate-methods → quality → full rspec, piped, serial) ran inside one
agent tool call and timed out. That was the "lefthooks fire and requests
time out" mess.

- Push is user-initiated, full stop. Deny at the permission layer (done);
  contract line in duet.md (the deny is the backstop; the contract stops
  the agent from even trying).
- Branch proliferation is a contract gap, not a permission gap — every
  branch-creating path already prompts. The rule: one session = one branch
  = one purpose, set by the user at kickoff. Tangents get one line of
  mention and route to the user's decision: fix here (commits separate
  fine later), open-items registry, or a fresh session. Never a branch,
  worktree, or PR mid-session. The false belief corrected: "branches can't
  contain multiple things" — the agent was making a granularity judgment
  that isn't its to make.
- `red` — the failure protocol (was gate-failure). Body: one line the
  failure, one line the proposed fix, go-ahead every time, no autofix, no
  provenance preface, no scope-widening refactor, failing check suspends
  the approved plan. Plus the hook-timeout discipline (Phase 1): a
  timed-out hook is a finding, not a flake — never blind-retry, never
  `--no-verify` or
  `LEFTHOOK=0` (skipping the check is worse than waiting), one line and
  wait. Phrased generically (hooks, check runners) — lefthook specifics
  stay in the other repos.
- commits widens to `git`: trigger "when git structure is at stake —
  commits, branches, pushes, PRs." Carries the existing commit protocol
  (exact files + message, question-tool approval, never widen) plus
  branch-on-request-only (`git co -b name` shape) and push/PR never
  proposed.

### Notes system — the two-layer notes model

The reframe that unlocked everything: tracked content is shared truth
(durable); ignored content is one checkout's working state (ephemeral).
The public tree never contains session ephemera.

- `.agent-notes/handoff.md` — fixed path, gitignored, overwritten at
  close-out. Always the current session state. Fixed address kills the
  failure-class the old design defended against: newest-file resolution,
  the archive-before-successor race, "point at the latest handoff must
  resolve" as a close-out check. The other repo's notes skill line "a handoff in
  sessions/ is the live plan until its successor exists" becomes
  structurally true. Name: handoff.md (names the artifact by role; no
  slugs, no variants).
- `.agent-notes/sessions/` — dated graveyard (YYYY-MM-DD-slug, born-date),
  gitignored, ~30-day expiry. NOT a permanent archive: the archive idea
  died because a handoff's durable content is what gets promoted; the rest
  is state narrative nobody reads in a week ("what is archive other than
  past sessions?" — nothing). The month window is a decay buffer for
  hitting issues.
- `.agent-notes/personal/` — scratch, gitignored. Named `local/` in the
  another repo's convention; renamed Nov 5 — the axis is personal vs shared, and
  `local` collides with git's clone-local meaning. `skills/personal/` is
  the incumbent word.
- Root notes — tracked living docs, the durable layer, frontmatter
  contract below.
- No archive/. Consumed means superseded (overwritten next close-out). The
  safety property moves into promotion discipline: unpromoted open items
  are a bug (existing rule) AND unpromoted decisions are a bug (new —
  decisions hide because they feel resolved once in the code; open items
  scream, decisions don't).
- Lifecycle = two acts. Close-out: promote open items and decisions to
  durable homes, copy outgoing handoff.md → sessions/, overwrite
  handoff.md with current state (one copy + one write). Opening: read
  handoff.md, verify its durable claims are durably recorded (promote
  anything that isn't — fix forward), work.
- Frontmatter (title optional/derived, status active|parked|done, summary
  required one-line): applies to DURABLE notes only — handoff.md,
  sessions/, personal/ exempt. summary kept but repurposed: the agent-facing
  triage line (how an opening act or weekly review decides which note to
  open at one line of context), NOT a human display field. Archaeology:
  the report never displayed summary in any version — the other repo's
  notes-skill claim that it does was false from birth; the user confirmed summaries are
  visual noise in a TOC. Tripwire: if summaries go stale or no agent acts
  on one, drop the field in a later pass — the consumer argument is
  currently speculative.

### Notes tooling — agency CLI and bin/notes

- `bin/notes` — standalone Ruby, stdlib only (YAML, Open3), ported from
  another repo's notes tooling, merged. Invoked through the
  agency CLI (RESOLVED Nov 6, user decision: one PATH entry point, `agency notes ...`,
  replacing a standalone `notes` link): `agency notes report` (grouped TOC:
  status sections, date, path — NO summary column, minimal by design),
  `agency notes stale` (90-day default, includes the personal/ section),
  `agency notes lint` (exit non-zero on invalid frontmatter — fixed Nov 6:
  the port printed problems but exited 0), `agency notes format` (autofill,
  review the diff). Pastel → plain ANSI. Lint scope narrows to root notes
  (durable layer only). Ruby floor: 4.0+ via any version manager (Nov 6,
  user decision: 2.6 compat rejected; in-file version guard rejected as dead code —
  old Rubies die at parse time — so the requirement is codified in docs
  only). `bin/agency install` links `agency` into `~/.local/bin`.
- `agency notes expire` — replaces the planned `gc` subcommand (RESOLVED
  Nov 6, user decision: "gc" names the mechanism, not the act, and flag-driven mass
  delete had no review). Lists `sessions/` docs older than N days (default
  30) with date + derived summary; deletion requires naming exact files
  (`expire -- <file> ...`), refused outside sessions/ — review by
  construction, no force flag. Agent flow: relay the listing (question-tool
  options with date/summary), user decides.
- Downstream delta: the four rake tasks become transition wrappers over
  `agency notes`, then die; lefthook pre-push calls `agency notes lint`
  directly. The TOC/staleness reporting moves to base wholesale (nothing in
  it was specific to the other repos — Pastel was the only coupling).

### Agents — write carve-outs

- research.md: scope superseded Nov 6 — see Phase 4's SUPERSEDED note
  (research/ category dir; ephemeral layers denied). Same carve-out
  pattern as review.md's ledger, same
  verify-on-first-run caveat. The distinguisher between research and plan
  is never write-permission — it's the interaction contract: research
  answers open questions (understanding is the deliverable), plan designs
  decided work (change-list is the deliverable). Both record their search
  in .agent-notes.
- Plan mode: project-relative. Plan docs are durable notes in the working
  project's `.agent-notes/plans/` (topic slugs, e.g. `plans/orchestration.md`),
  NOT in a global opencode directory. Scratch and destination in one place,
  per-project, visible to notes tooling. The global `~/.opencode/plan`
  dissolves (or stays as legacy fallback). See plan-mode mechanism below.
  RESOLVED Nov 6 (user decision): category directory beats the earlier `plan-*.md`
  slug-at-root convention — permission boundaries align with directories
  exactly, while filename prefixes are approximate under V2 globs (`*`
  crosses `/`), and the directory gives plans a real home as
  `.agent-notes/` grows categories (sessions/, personal/, plans/).

### Plan-mode mechanism (verified against source, Nov 5 2026)

opencode's plan mode is a plugin: `packages/core/src/plugin/plan.ts` in the
source checkout at tmp/opencode (shallow clone, dev branch). It injects the
"You are in Plan mode" reminder on switching to any agent with ID `plan`
(hardcoded text) and pushes permission rules: `edit *` deny + `edit
~/.opencode/plan/*` allow. The directory is hardcoded (`path.join(
global.home, ".opencode", "plan")`), no config key. The "Cannot use write
outside the Plan directory" error is a message rewrite over a normal
permission denial (`tool.hook("execute.after")` swapping BlockedError
messages) — the real gate is the ruleset.

Empirical finding: the `agents.plan` config block REPLACES the built-in
edit rules rather than appending — proof: a write to `~/.opencode/plan/`
itself was blocked after the config block landed, meaning the built-in's
plandir allow was gone and the block's deny-* matched. Consequence: config
fully controls plan-mode write permissions; the `.agent-notes/*` carve-out
is live (this doc's own write is the proof); the plandir allow is
intentionally NOT restored (project-relative is the design).

What config cannot change: the injected reminder still says "write them
only in ~/.opencode/plan". Options, in order of preference: (a) small user
plugin injecting a corrective reminder on plan-agent switch (plugins can
hook the same events); (b) override the agent's `system` field and live
with the competing reminder; (c) tolerate the conflict — the reminder's own
"unless the user explicitly asks you to" clause gives explicit user
direction precedence, which is how this session worked. RESOLVED Nov 5:
   custom-agent sidestep — see the forks section.

### Saved permission approvals (mechanism)

opencode's "always" reply on a permission ask is NOT session-scoped: it
writes a permanent row to the harness DB (`permission` table, keyed by
project), layered into every future session's ruleset before the ask
fires. Saved allows override config asks indefinitely; config *denies*
still win (cannot be overridden by any allow). A broad saved allow can
silently defeat a whole tuned ask layer — audit the table periodically,
and treat "always" as a deliberate, per-project, permanent grant.

### Workflow-doc restructure (machine-local)

Lineup section reorganized lifecycle-first (start → do → react → punt →
close). Vocabulary rule at top. Overnight marked parked, not deleted from
history. Plan mode added to the lineup (project-relative plan docs, the
research/plan distinguisher, the replace-vs-append permission semantics).
The pruning and workflow-design references stay in the machine-local
layer. Layering rule stated: overrides carry deltas, never re-state
the base — the drift lesson from the two divergent handoff lifecycles.

## Phase 0 — config (DONE, Nov 5 2026)

opencode.jsonc, two edits, file re-parsed clean (JSON5):

1. `git push*` moved from the ask block to the catastrophic-denies section,
   start-anchored, comment documents rationale + the accepted
   `git -C elsewhere push` gap.
2. `agents.plan` block: `edit *` deny, then `.agent-notes/*` allow.
   Empirically verified replacing (not appending) the built-in edit rules —
   see plan-mode mechanism above. Verification item RESOLVED for the
   permission half; the reminder-text half resolved via the fork-2
   custom-agent sidestep (see forks).

## Phase 1 — renames and merges (~/.agents) — DONE Nov 5

Files:

- skills/check-in/ → DELETE (dissolved into whoa).
- skills/whoa/ ← NEW: body = check-in's protocol; description = "Use when
  the user says whoa, or the agent is running off, deciding without
  asking, or overstepping — stop and surface the decisions."
- skills/missing-software/ → skills/missing-tool/ (rename dir + name).
- skills/session-handoff/ → skills/handoff/ — body REWRITTEN per Phase 2
  (rename and rewrite are one operation).
- skills/commits/ → skills/git/ — widened trigger + branch/push/PR
  protocol per git policy.
- skills/gate-failure/ → skills/red/ — body rewritten in check/failure
  language (two "gate" mentions out), hook-timeout discipline added.
- commands/whoa.md — update: loads the whoa skill.
- commands/re-anchor.md + remind-gate.md → merged into commands/drift.md;
  remind-gate deleted.
- commands/step-gate.md → move to another repo's agent content
  (verify opencode discovers project-level .agents/commands first; if not,
  keep global with a comment).

Verification: grep the tree for stale references (check-in,
session-handoff, missing-software, gate-failure, commits skill, re-anchor,
remind-gate). In-agency ripples: README.md line 17 (re-anchor → drift),
specs/SKILL.md's reference line (rewrites around "the red skill").
Other-repo references are their own ripple, not this phase.

## Phase 2 — notes system in agency — DONE Nov 5

Files:

- .agent-notes/ — CREATE (plans/orchestration.md and handoff.md already
  exist — the plan doc from this session's planning, handoff.md as the
  first artifact of the new lifecycle, written under its own design before
  the skill rewrite lands). Add sessions/ and personal/ dirs.
- .gitignore — add: `.agent-notes/handoff.md`, `.agent-notes/sessions/`,
  `.agent-notes/personal/`, `tmp/` (the opencode source checkout lives in
  tmp/opencode; scratch per shared AGENTS.md).
- skills/handoff/SKILL.md — REWRITE around the two-act lifecycle, fixed
  handoff.md, sessions/ graveyard with 30-day expiry pointing at bin/agency
  gc, no archive, promotion covers open items AND decisions,
  project-parameters shrink (registry path + notes-tracked policy).
- skills/agent-notes/SKILL.md — NEW: format contract. Frontmatter fields
  (title optional/derived, status enum, summary required as agent-triage
  line — explicitly NOT displayed in report), naming rules (dated for
  point-in-time dirs, undated root docs), bin/notes usage, lint scope =
  root notes only. Base for the other repos' delta.
- bin/notes — port per tooling decisions.
- bin/agency — add `gc` subcommand; update usage text.
- AGENTS.md (agency) — the two pointer lines per the skill's
  project-parameters requirement (pointer + registry path; the meta-repo's
  registry lives in the machine-local workflow doc).
- README.md (agency) — layout gains .agent-notes/ and bin/notes; "using
  it" gains gc/notes commands.

Verification: bin/notes lint/report/stale against the fresh .agent-notes;
gc dry-run; gitignore effect (handoff.md absent from git status).

## Phase 3 — git policy prose — DONE Nov 5 (duet.md behavior-only; slogan skipped, lives in skills/git)

- agents/duet.md — git-structure section: one session = one branch = one
  purpose (set by user at kickoff); never branch/worktree/PR mid-session;
  tangent → one-line mention → user routes (fix here / registry / fresh
  session); push and PRs never proposed.
- skills/git/SKILL.md — operational protocol (if not done in Phase 1):
  branch on request only, `git co -b name` shape, push/PR user-initiated,
  tangent routing.

## Phase 4 — research carve-out — DONE Nov 6

- agents/research.md — singular `permission:` → plural `permissions:`
  array; `edit *` deny + `.agent-notes/*` allow; verify-on-first-run
  comment (review.md precedent). Body: search records and analysis docs
  land in .agent-notes/ as durable root notes (promote, don't park).
  `.agent-notes/*` (not plans/-narrowed like fork 2's planning agent):
  research writes search/analysis docs across the durable layer, not just
  plan docs. Verified via `opencode debug agents` from a fresh process —
  `edit *` deny then `.agent-notes/*` allow in the merged chain. Caveat
  discovered: a long-running opencode server (Nov 5 instance) serves
  agent config from its load-time cache; debug/agents reads inside that
  project showed stale content until the server cycle.

  SUPERSEDED Nov 6 (user decision, same day): `.agent-notes/*` included the
  ephemeral layers (sessions/, personal/, handoff.md) — a hole. Taxonomy
  resolved as category-dirs-by-writer: root = duet/harness-written
  durable notes; plans/ = planning's intent docs; research/ = research's
  understanding docs; sessions/, personal/, handoff.md ephemeral and
  denied to category agents. Research agent re-scoped to
  `.agent-notes/research/*`; why-agents-edit.md migrates to research/.
  bin/notes ripple: durable-note scan should cover category dirs
  (plans/ + research/), currently root-only.

  IMPLEMENTED Nov 6 2026 per `.agent-notes/plans/research-category.md`
  (designed live by the planning agent in-session): research.md re-scoped
  and verified via `opencode debug agents` from a fresh cwd (frontmatter
  lands last: `edit *` deny → `.agent-notes/research/*` allow);
  why-agents-edit.md migrated; bin/notes scans the durable layer
  (root + plans/ + research/) for report/stale/lint/format — lint widened
  to category dirs per the user's call, clean on first run. Live boundary test
  VERIFIED Nov 6 (research switch): research/ write succeeded
  (research/notes-permission-mechanics.md — consolidated permission-chain
  findings), deliberate probe outside research/ denied by the frontmatter
  `edit *` deny, no reminder noise on the switch. Window closed.

## Phase 5 — workflow-doc restructure + ripples

  AGENCY-SIDE DONE Nov 6 2026: the machine-local workflow-doc restructure
  landed (vocabulary
  rule, lifecycle-first lineup against the on-disk agents/skills/commands,
  plan-mode entry with the permission semantics, layering rule, overnight
  history compressed per the user's call — yOLO-removed entry, overnight lineup
  item, overnight-plan skill, and the 90-line run section folded into one
  history block; the registry section survived verbatim). Ripple sweep
  verified clean: agency tree, README, and skills/personal carry no stale
  renamed-skill references; step-gate already carries the fork-4
  dependency comment; the only per-project rows left (weekly-review's
  the per-project notes-layout notes) belong to those repos, not
  agency. Scope decisions Nov 6 (user): other-repo work stays out of
  this pass (own session); the research/ shape keeps its own window.

- Workflow doc (machine-local) — DONE (see above).
- Sweep ripples — DONE, verified clean.
- Downstream work (SEPARATE repo pass, still open): the notes skill shrinks
  to delta; the lefthook pre-push notes-lint → `agency notes lint`
  (fork 1 resolved: gem-side notes-lint moves out entirely, no ported
  copy, no transition wrappers — exact shape per a note in that repo
  note); AGENTS.md pointer line session-handoff → handoff;
  sessions/ committed-docs migration decision
  (their call, per-project parameter); step-gate gem-side relocation;
  local/ → personal/ rename (agency renamed
  Nov 5; their dirs are the migration tail).

## Open forks (decide at execution)

1. **Lint canonical home** — RESOLVED Nov 5: canonical in agency
   (`bin/notes`), full stop. A note in that repo moved the
   move the gem-side notes-lint out entirely — no ported copy, no
   transition wrappers, so the drift concern doesn't arise. Downstream
   work (removal + lefthook retarget) is tracked by that note, not this
   repo.
2. **Reminder-text fix** — RESOLVED Nov 5, IMPLEMENTED Nov 6: custom-agent
   sidestep. The built-in reminder is hardcoded in core and self-healing
   (plan.ts's `session.hook("context")` reconciler re-appends it when
   missing), so it cannot be removed or overridden by config or plugin.
   But the entire plugin keys on exactly one agent ID (`plan`). Fix: the
   file-defined agent `planning` (agents/planning.md) — different ID, mode
   primary, prompt carries the plan-mode contract (topic-slug plan docs in
   the project's `.agent-notes/plans/`), permissions edit * deny →
   .agent-notes/plans/* allow → question * allow (tightened Nov 6 from the
   original .agent-notes/* allow, per the plans/-directory decision). The built-in `plan` agent
   stays defined but hidden (`agents.plan` config gains `"hidden": true`)
   — same permissions, stale reminder, out of the picker/tab cycle
   (TUI filters !agent.hidden). Name rationale: single word to match the
   duet/research/review/scout lineup; research → planning → duet → review
   reads as the lifecycle. Verified via `opencode debug agents`: planning
   discovered with correct mode/prompt/rules, plan hidden true, config
   reloaded clean. Costs accepted: generic permission-denial message
   instead of the plan-dir rewrite, no enter/leave reminder bookkeeping
   (not needed), TUI plan shortcut still targets the hidden built-in.
   Outstanding: first live planning session — confirm no plan reminder
   and .agent-notes/ writes work (verification item below).
3. **Agency's open-items registry path** — RESOLVED Nov 5: the
   machine-local workflow doc, per the lean. Recorded in this repo's
   AGENTS.md "Notes and handoffs" section.
4. **step-gate project-commands discovery** — RESOLVED Nov 5 against the
   opencode source (tmp/opencode checkout): commands are scanned only in
   config Directory entries — system/global config dirs and project
   `.opencode/{command,commands}/` roots. Project `.agents/` dirs feed the
   skills compatibility plugin only, so the plan's assumed target
   (another repo's `.agents/commands/`) would never load. Resolution: step-gate
   stays global with a dependency comment. It is NOT specific to one repo —
   the quality-review skill ships via a gem to a family of apps, and is
   meaningless outside them. The durable home is gem-side (shipped
   alongside the skill), not per-app — per-app copies are the drift the
   family's shared AGENTS.md warns about. RESOLVED Nov 6 2026 by
   absorption: the re-entry behavior folded into the quality-review
   skill's step 3 (skip to the group walk when
   findings are already on screen); the agency command deleted, no
   separate step-gate skill — a second name for a sub-step fails the
   vocabulary rule. Commands could never load gem-side anyway (fork's
   discovery finding), which is what made absorption the only clean shape.

## Verification items

- `git push*` deny: confirm start-anchored matching catches common shapes;
  `git -C elsewhere push` gap accepted, revisit only if bitten.
- Project-level `.agents/commands/` discovery (step-gate).
- review.md ledger carve-out unaffected by permission-layer changes.
- Plan-mode reminder text: RESOLVED and implemented by fork 2
  (`planning` agent + hidden built-in `plan`). Remaining check: first
  live planning session — no plan reminder injected, .agent-notes/
  writes work, built-in untouched when hit via TUI plan shortcut.

RESOLVED Nov 5: plan-mode `.agent-notes/*` write — config block controls
plan-mode permissions (replace semantics), proven by this document's own
write.

## Explicitly out of scope

- overnight agent file — parked Nov 5 (user: don't want it now). BUILT Nov 6
  as `yolo` with a different entry (in-session agent switch, no plan doc, no
  runner flags) after the user asked for night runs back — see the
  workflow doc's lineup
  + the yolo open items. This bullet is history, not a live constraint.
- Renaming duet/research/review.
- ~/.config/opencode/skills/ empty dir — the user's opencode-local skills slot,
  stays.
- Any other-repo migration — separate pass, listed in Phase 5.