---
description: Use when git structure is at stake — commits, branches, pushes, PRs.
---

Propose the exact files and the exact message, then wait for approval of that exact proposal. Never widen it ("while we're in here").

History-mutating operations (amend, reset, rebase, cherry-pick, force-push) are never covered by a prior approval — each needs its own. Before any amend, name the target commit and confirm it is the intended one; HEAD is where unrelated recent commits live.

An approval covers one attempt. When any git operation fails, the sequence stops — show the raw failure and wait for direction; a retry is a new proposal.

Agents never push. `git push` (and any remote-mutating git operation) is the developer's hands only, unconditionally — deliver the branch name and stop. The developer pushes and opens PRs.

The default is the current branch — `main` included. An agent never puts new work on a branch it did not ask for: no `git co -b`, no `git switch -c`, no worktree, no rename-and-move, no tool that lands commits somewhere other than the branch the session started on — the rule covers the outcome, not the command spelling. Every branch creation requires the developer's explicit word addressed at branching, asked and approved first, every single time. If a branch seems needed, that is its own stop-and-ask before any work begins.
