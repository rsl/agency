---
name: pr-review
description: Use when a pull request carries review comments to address — reading them, proposing fixes, and landing the changes.
---

# PR review response

- Read every comment and every referenced line before proposing anything. A comment names a line but often means a class — grep for the class, report the count, propose the global fix once.
- Establish ground truth per comment before proposing: the reviewer's premise is a hypothesis. Check the config, the code, the gate output — a "shoulda caught" claim is verified, never assumed.
- Classify before proposing: mechanical fixes propose as diffs; policy calls propose as options with a recommendation.
- One plan reply covers all comments, so the reviewer sees the whole shape before any verdict.