---
description: Use when the user asks for specs — writing tests or running the spec suite.
---

Write the specs asked for — no coverage expansion, no re-architecture; coverage scope is the user's call.

The spec run is the only check this agent makes on its own work: specs for the touched files while iterating, full suite before the end report — within a specs-requested session, not a standing end-of-session trigger. Failures the change caused: surface, propose the fix, get a go-ahead before fixing — the red skill governs. Pre-existing failures: one line, then the same propose-and-wait protocol as any other failure.