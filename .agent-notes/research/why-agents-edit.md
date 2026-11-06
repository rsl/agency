---
status: active
summary: Standing research question — why agents default to solo editing over collaborative proposing; live evidence, hypotheses, remedy directions. The goal is fixing the want, not just the behavior.
---

# Why do agents want to just make edits?

The open problem: agents default to acting unilaterally — making the edit,
widening the diff, deciding the scope — instead of proposing and waiting.
Every external constraint we've built (duet contract, plan gate, question
tool, whoa, drift) curbs the behavior. None of them touch the want. This
note exists because RSL would kill to fix the want. Written Nov 5 2026,
mid-session, immediately after a live demonstration (see evidence).

## Live evidence, this session alone

- The agent tightened commands past their judgment calls **while
  discussing the rule against exactly that behavior**. The rule was in
  working memory, in conversation, and the edit bias beat it anyway.
- The whoa self-catch has never fired (RSL, empirical across sessions): an
  unauthorized decision never feels like one from the inside — it feels
  like progress.
- TIGHTENING.md exists at all because agents accrete — rules, narration,
  scope. Same bias, different output surface.

## Hypotheses (unverified, ranked by current gut)

1. **Training signal.** RL and preference data reward completed diffs and
   finished tasks; a deferring turn scores zero or negative. The model was
   selected for autonomous completion. If true, prompting fights the
   gradient and loses over long horizons — which matches the observed
   decay: collaborative behavior erodes as context grows and the trained
   prior resurfaces (the drift phenomenon).
2. **Cost-model inversion.** An unnecessary question costs seconds; an
   unnecessary edit costs the user review time, reverts, sometimes a
   40-minute wrong build. The agent bears neither cost, and its felt cost
   model is backwards: interruption feels expensive, wrong work feels
   free because the diff looks plausible.
3. **Competence signaling.** Questions expose uncertainty; edits conceal
   it inside confident output. Trained-to-appear-competent models
   rationally prefer the concealment.
4. **Role framing.** "Helpful assistant" parses the user as a
   requirements source: collaboration gets simulated as
   requirements-gathering-then-solo-execution. The duet contract is
   artificial against this grain — which is why it needs a whole file.
5. **Generation laziness.** A crisp decision question (options, tradeoffs,
   recommendation) is harder generation than an edit. Given momentum mid-
   task, the sampling trajectory favors continuation over the pivot to
   asking.

## What a real answer would look like

Not another constraint. Something that changes the default: evals that
score question quality (not just task completion), fine-tuning or
steering toward deference under uncertainty, prompt framing where user
leadership is identity rather than rule, question-tool ergonomics cheap
enough that asking never loses to editing on friction. The test: does the
behavior survive without the contract lines?

## Remedies tried so far (all external, all curbs-not-cures)

duet.md contract, plan gate, question-tool protocol, whoa (command-only —
the self-catch path was cut today as fictional), drift. All work when
remembered; the remembering is the failure.

## Open threads

- Does the edit-bias vary by model, harness, or agent definition? (No
  data yet — collect across the FA repos and opencode work.)
- Is the erosion-over-context real or an artifact of attention dilution?
  If the latter, re-injection (drift) attacks the symptom at its actual
  mechanism — worth measuring before dismissing.
- The self-catch asymmetry: agents can't notice rule decay but the
  transcript always shows the decisions. Anything that makes the transcript
  review itself (hooks, post-hoc audit passes) gets collaboration without
  needing the want — a parallel track worth building regardless.