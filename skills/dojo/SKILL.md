---
name: dojo
description: Use when the user wants to learn, practice, review, or brush up on concepts from the codebase.
---

Interactive learning sessions taught from this repo's real code — never toy examples.

## Session start

1. **Calibrate by probing, not asking.** Never ask skill level — self-report is a bad dial. Open at the hardest layer the material offers and adapt down on a miss. One goal question ("brush up vs deep dive") suffices when the request isn't obvious.
2. **Ask the mode.** Offer Explain, Quiz, or Drill via the question tool.
3. **Establish the source material.** Default: the current changeset (`git diff main`, or live staged work). The user may name a file, symbol, directory, PR, or domain — for a concept, pick the repo's most instructive real code. Small named target: scope it inline — read the files yourself. Broad target (whole changeset, a domain, drill mode): delegate a survey to the scout subagent for a shortlist — file:line, the concept, why instructive. Read the finalists in the main thread; teach from firsthand code, never from the summary.

## Modes

**Explain** — walkthrough of the source material, concept by concept: show each real hunk, name the concept, explain the why. Order concepts structural to subtle; close with the two or three ideas worth remembering.

**Quiz** — Socratic, one question at a time: show a real hunk, ask about it, wait for the answer. Grade honestly; dig one level deeper on the why before moving on. Misses are teaching moments — walk the reasoning, not just the verdict.

**Drill** — pick one concept, find every occurrence across the repo (CodeGraph first where indexed), and present as a series — calling out variations and outliers that break the pattern.

## Ground rules

- Real code only, cited as `file:line`.
- Teach house conventions as house conventions — cross-reference the relevant skill when one exists.
- Match depth to calibration — why and edge cases for the experienced, mental model first for juniors.
- One concept or question at a time, never a wall of text; end each exchange with a natural next step.
