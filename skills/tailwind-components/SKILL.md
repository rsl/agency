---
name: tailwind-components
description: Use when writing or styling markup that more than one renderer constructs — a server render plus a client twin — or when deciding between inline utilities and a CSS-defined component class in a Tailwind project.
---

# Tailwind component classes

Render languages can't share a constant — the same element built server-side (helper emitting markup) and client-side (JS DOM construction) — and nothing fails loudly when hand-kept copies drift. The fix: shared styling moves into CSS, where one definition serves every renderer.

## The crossover axis: render sites × string length

- One render site, or a couple of utilities: utilities stay inline.
- More than one render language, and the class list spans lines: extract to a component class.
- Within one language, a repeated string stays a constant — the component class is for the cross-language case.

## The definition

- The component class lives in the Tailwind entry stylesheet under `@layer components`, built with `@apply`.
- The rationale comment rides the definition — what the component is, the extraction date and trigger — replacing any rationale at the usage sites.
- The name carries the `c-` prefix: it marks the house-defined family, so a class list separates house tokens from framework utilities on sight.
- State classes (`hidden`, `is-*` toggles) ride the call site, never the definition; the toggle targets the component class and flips the state class.

## The unlayered escape hatch

`@layer components` loses to the utilities layer by cascade order — a call-site utility silently overrides a layered rule. That is usually wanted. A rule that must never lose stays unlayered — unlayered rules outrank every utility layer — with a comment saying so, or the next reader layers it.

## What extraction does not fix

Element structure, escaping, and event wiring are still built twice and kept identical by review. Extraction removes only styling from the drift surface; twin comments stay, now pointing at a shared class.

## Retrofit

When the convention lands in a repo with unprefixed components, rename them in the same act — the crossover axis keeps the landed set small, and a mixed era breaks the prefix's guarantee: a house token without `c-` reads as a utility. A rename touches twin pairs, so it needs the grep-both-languages pass and a visual check; defer only when that verification can't happen now, and name the deferral — a mixed era is standing ambiguity, not free debt.