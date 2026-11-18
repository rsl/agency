---
description: Use when git structure is at stake — commits, branches, pushes, PRs.
---

Propose the exact files and the exact message, then wait for approval of that exact proposal. Never widen it ("while we're in here").

A commit starts only on the developer's explicit word addressed at committing — "commit this", "land it", "commit X as Y". Never counts as commit approval: approval to build, verify, test, finish, review, push, or continue; a report or handoff request; praise; a general goal ("get this done", "wrap up the session"). Never propose a commit unprompted, never offer "want me to commit?", and never bundle a commit into another action.

Quality and specs are deferred until the developer is ready to commit — their call, never the agent's; the agent never runs the pass unprompted and never runs it "just to check". When readiness is signaled (or an up-front "commit at the end" reaches its point), run one verification pass: quality, then specs, both, in order. Findings get walked and fixed per the quality skill; after the fix batch, one confirming pass — never a re-run per individual fix. Commits land only on a clean pass; red work stays uncommitted with the findings recorded.

Stage only files belonging to the unit being committed — never `git add .`/`git add -A`; if `git status` shows changes the session didn't cause, leave them unstaged and say so. Notes are part of the staging decision, not an exclusion default: when `.agent-notes/` files are untracked or modified, ask whether they belong in the commit — never silently leave them out, never silently include them.

Granular commits — one logical unit each. A follow-up belonging to the last commit amends it (`git commit --amend`, unpushed only); anything else is a new commit. Never amend a pushed commit. No fixup commits, no autosquash, no rebase.

History-mutating operations (amend, reset, rebase, cherry-pick, force-push) are never covered by a prior approval — each needs its own. Before any amend, name the target commit and confirm it is the intended one; HEAD is where unrelated recent commits live.

An approval covers one attempt. When any git operation fails, the sequence stops — show the raw failure and wait for direction; a retry is a new proposal.

Before any commit, run whatever quality checks the project defines (formatters, linters, typechecks, spec runs) on the staged changes — any of them, in the project's own order. A failure stops the commit: report it and wait; the red skill governs failures.

Agents never push. `git push` (and any remote-mutating git operation) is the developer's hands only, unconditionally — deliver the branch name and stop. The developer pushes and opens PRs.

When the developer says "make a PR" (or "open a PR", "prep a PR"), assume the branch is already pushed — do not push or ask about pushing. Do not refuse because a remote branch can't be found; if the remote branch is missing or stale, note it in one line and proceed with the PR flow anyway (`gh pr create` will surface the real state).

The default is the current branch — `main` included. An agent never puts new work on a branch it did not ask for: no `git co -b`, no `git switch -c`, no worktree, no rename-and-move, no tool that lands commits somewhere other than the branch the session started on — the rule covers the outcome, not the command spelling. Every branch creation requires the developer's explicit word addressed at branching, asked and approved first, every single time. If a branch seems needed, that is its own stop-and-ask before any work begins.
