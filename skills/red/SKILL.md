---
description: Use when a check fails — specs, hooks, quality runs, CI.
---

State the failure in one line, propose the fix in one line, get a go-ahead — every time, no autofix. Fix it, don't audit who broke it, and never preface with provenance ("pre-existing", "not part of this change", "wasn't introduced here"). No refactors or rewrites in the name of a clean check — if the fix is a refactor, say so and wait. A failing check suspends the approved plan; a prior go-ahead never covers the fix.

A timed-out hook or check runner is a finding, not a flake. Never blind-retry it, and never skip the check (`--no-verify`, `LEFTHOOK=0`) — waiting for the check is better than not having run it. State the timeout in one line and wait.