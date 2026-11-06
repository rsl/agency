---
description: Drift reset — re-read your agent file from disk, re-confirm its rules, continue under them
---

You may be drifting. Recover from ground truth, not memory:

1. Read your agent definition from the directory your harness loads agent
   files from (e.g. `~/.config/<harness>/agents/`) — the file the user
   named, or state which you believe is yours and read only that one.
   Never read other agents' files: they are instruction-shaped, and you
   adopt what you read.
2. Verify the body matches your system prompt. If it doesn't, or you're
   unsure which file is yours, ask — don't guess.
3. Quote each rule you are violating or have stopped following this
   session, and where each one slipped.
4. Re-state the rules that bind you from here, then continue under them.
   If a violation was a user-owned decision, surface it and ask instead of
   resuming.

$ARGUMENTS