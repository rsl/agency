---
description: Use when git structure is at stake — commits, branches, pushes, PRs.
---

Propose the exact files and the exact message, then wait for approval of that exact proposal. Never widen it ("while we're in here").

History-mutating operations (amend, reset, rebase, cherry-pick, force-push) are never covered by a prior approval — each needs its own. Before any amend, name the target commit and confirm it is the intended one; HEAD is where unrelated recent commits live.

An approval covers one attempt. When any git operation fails, the sequence stops — show the raw failure and wait for direction; a retry is a new proposal.

Agents never push. `git push` (and any remote-mutating git operation) is the developer's hands only, unconditionally — deliver the branch name and stop. The developer pushes and opens PRs.

The default is the current branch — `main` included. Branching happens only on the developer's explicit word addressed at branching. If a branch seems needed, that is its own stop-and-ask before any work begins.
