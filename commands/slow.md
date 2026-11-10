---
description: Diagnose slow or costly responses — find runaway opencode/zed sessions eating tokens
---

The question is usually "who is spending tokens right now", not "how fast is the model". Work through the probes, then report — never kill anything without explicit approval.

$ARGUMENTS

## 1. Find live consumers

List running processes for opencode, zed, and omlx-server: `ps aux | grep` (exclude the grep itself). For each, capture pid, elapsed time, and CPU. Long-lived opencode or zed agent processes are the suspects.

## 2. Identify the sessions behind them

- Log dir: `~/.local/share/opencode/log/` — find the newest files, check whether lines are still advancing (compare line counts a few seconds apart). Advancing logs mean active requests.
- Session store: `~/.local/share/opencode/opencode.db` (sqlite). Recent rows identify which project and session each process is serving.
- For zed: check zed's own log dir under `~/Library/Logs/Zed/` for active agent sessions.
- Token burn: grep recent log lines for token-count or usage fields; if present, estimate per-session burn rate (tokens per minute) from the timestamps.

## 3. Check the providers

Read provider baseURLs from `~/.config/opencode/opencode.jsonc` — resolve `{env:VAR}` names only, never print key values. For each omlx server: `curl /v1/models` with the env key to confirm it is up and list loaded models. An idle server is fine; the concern is which sessions are actively streaming against it.

## 4. Report

For every live session: pid, owning app, project, age, evidence of activity (advancing log lines, timestamps, burn estimate if recoverable). Then a verdict: runaway / active-but-expected / idle. If something looks runaway, propose the exact kill command(s) and stop — killing is the developer's explicit call.