---
description: Walk the interactive offense-group flow — show before/after diffs per group, discuss, apply on approval, re-run, confirm
---

<!-- Depends on the fa-quality-review skill, gem-distributed by unifractured
     to the FA Rails apps — meaningless outside that family. Kept global
     deliberately: it runs from any app checkout. Durable home is gem-side
     (shipped alongside the skill), not per-app; see plans/orchestration.md
     fork 4. -->

Follow the fa-quality-review skill's step 3 exactly for the current gate findings: one offense group at a time, each introduced with concrete before/after proposals (no edits applied), discussion before application, re-run the gate and confirm the group cleared before the next. New findings surfaced by a fix are their own group — verbatim report first.

$ARGUMENTS