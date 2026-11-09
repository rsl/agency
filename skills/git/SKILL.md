---
description: Use when git structure is at stake — commits, branches, pushes, PRs.
---

Propose the exact files and the exact message, then wait for approval of
that exact proposal. Never widen it ("while we're in here").

Branch on request only: `git co -b <name>` from an up-to-date `main`, no
other arguments, ever — no start-point argument. A remote-tracking start
point (`git co -b <name> origin/main`) silently sets the new branch's
upstream to that remote branch, and plain `git push` follows the upstream,
not the branch name — it would target `main` without ever saying so.

The default is the current branch — `main` included. Branching happens
only on the developer's explicit word addressed at branching. If a
branch seems needed, that is its own stop-and-ask before any work
begins.
