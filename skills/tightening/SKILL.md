---
description: Use when the user asks to prune, tighten, or slim agent files, rules, skills, or notes — the periodic accretion pass.
---

# Tightening

Agent files only grow; each war story adds a rule and rules accrete narration. First step of any pass: the scope question — use the question tool to ask which surface this pass covers — the always-loaded instructions, an agent file, a skill, or the notes tree — unless the caller already named it. One surface per pass; finish it before starting another.

## Classify every rule before touching it

1. Constant behavior → standing rule (always-loaded file).
2. Rare event with a discrete pause point (a check fails, a tool is missing, a session ends) → skill. Description states the trigger; body states only the deviation from default competence.
3. Rare event with no pause point → cut. A standing rule taxes every session for a rare one; re-add with the war story if it bites.
4. Judgment call made mid-investigation (no trigger moment — the model won't go skill-shopping while reasoning) → standing rule.

## Placement is half the decision

- A rule that must govern every write (formatting, naming, attribution) cannot be a skill: skills are lazy-loaded and miss the moment. It belongs in an always-loaded file, in the section whose identity fits — and stays one bullet.
- A skill fires on a trigger moment; anything that must already be in context before the trigger never reaches it.
- Shared base vs local delta: base files carry portable rules; local config carries machine specifics. A delta never re-states the base.
- Sub-steps don't earn new names. A behavior that is step N of an existing skill folds into that skill's body; a second name for a sub-step fails the vocabulary rule (no name is protected by precedent — re-earn every name against the current lineup).

## Rules for rules

- State the behavior, cut the rationale — rationale lives in the design reference. Keep the why only when it defines the failure shape.
- One concrete anchor per abstraction; a second example of the same abstraction is restatement.
- Examples calibrate (set magnitude) or get read as allowlists; pair one invariant phrase with examples as illustrations.
- A rule must be checkable in output — "prefer 1, cap 2" survives; "prefer acting over deliberating" can't be followed or violated.
- Declarative, not rhetorical.
- Audit for cross-section restatement before compressing anything — the worst bloat is the same rule stated twice in different sections.
- Format rules fold with their kind; a style rule and a protocol split.

## Skills

- Description = trigger, nothing else. Policy is body content.
- Body = deviation from default competence only — models already check for an alias or PATH issue.
- One-rule skills skip the header; name and description carry identity.

## Notes and registries

- Registries hold live items only; resolved entries leave on resolution — file history carries them, not the registry.
- Decision records (plan docs, research notes) keep their why; live surfaces carry pointers, never re-statements.

## Publication boundary

- No client, project, repo, or user names; no relationship sketches or infrastructure specifics that triangulate private work — genericize to "another repo" or cut the clause, including in historical records.
- `skills/personal/` is the personal layer and exempt — names and personal context belong there by design.
- Uncommitted history ships at first push; scrub before it compiles.

## Process

- Read the reference material and the actual config/files before proposing cuts — most bad proposals die on a fact one read would surface.
- The pass edits a copy, never the live file. Flow, in order: copy the target surface into `.agent-notes/drafts/` → edit the copy → review the edits in the copy. Draft names mirror the source path with `/` replaced by `-` — `skills/handoff/SKILL.md` drafts as `drafts/skills-handoff.md` — one naming rule, every pass, so drafts land predictably instead of differently each time.
- All refinement happens on the draft. The review is the itemized draft-vs-live diff — each cut or move one numbered line, with the reason it dies — and the walk settles every item on the draft, each shown verbatim (original and final text as quoted before/after blocks, never a prose summary) with its verdict through the question tool. Vetoes correct the draft. The live file is written exactly once, only after the walk ends with every item settled — never mid-pass, never per item.
- When the pass ends, prune the draft; git history carries the record.
- Count rules before and after; rules survive 1:1. A rule that dies was a restatement — say so and let the owner veto.
- When a pass ports details into skills, audit the receiving skill for redundancy with the source — cross-file restatement is the same sin as in-file restatement.