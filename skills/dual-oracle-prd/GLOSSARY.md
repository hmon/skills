# GLOSSARY

## Prime

Send a system prompt to an oracle before any question is asked. The prime prompt dictates three non-negotiable elements: **response format** (conclusion-first, terse), **purpose** (maximum depth and precision), and **power level** (use maximum power, no holding back). The Laborer generates one prime prompt and sends it identically to both Oracle A and Oracle B. No oracle sees the other's prime.

## Grill

Send brief context + one focused question to a single oracle. The context is ≤ 3 sentences. The expected output is a terse, high-signal answer: conclusion first, then ≤ 2 sentences of reasoning. No filler, no hedging. The Laborer grills Oracle A and Oracle B independently with identical prompts.

## Triage

Compare the two oracle answers side-by-side and classify them into exactly one bucket:

- **Aligned** — same conclusion, same reasoning.
- **Conflict** — same facts, different conclusion.
- **Drift** — different facts, different assumptions, or talking past each other.

Triage is a binary-or-ternary gate: Aligned means done; Conflict or Drift means converge.

## Converge

When oracles are not Aligned, write a secondary prompt that surfaces the exact disagreement and forces each oracle to defend their position in ≤ 2 sentences, then state what would change their mind. The secondary prompt is sent to both oracles independently. After responses, return to Triage. Maximum 3 convergence rounds.

## PRD

Product Requirements Document. The single artifact produced by this skill. It contains: Decision (1 sentence), Reasoning (2–3 sentences), Scope (boundaries), and Deadlocks (if any). Hard cap: 300 words. The PRD is reviewed by both oracles before finalization.

## Laborer

The agent running this skill — the coding agent with local code access. The Laborer is the sole communication channel between Oracle A and Oracle B. Oracles never see each other's outputs. The Laborer primes, grills, triages, converges, drafts, reviews, and finalizes.

## Oracle

An expensive external model invoked for high-depth reasoning. Oracles are cost-constrained, so every interaction must be brief and high-signal. Oracles do not communicate with each other; all inter-oracle traffic routes through the Laborer.

## Aligned

Triage bucket: both oracles reach the same conclusion via the same reasoning. The Laborer may proceed to Draft PRD.

## Conflict

Triage bucket: both oracles agree on the facts but reach different conclusions. Requires Convergence.

## Drift

Triage bucket: oracles operate from different facts, assumptions, or scopes. Harder to resolve than Conflict because the gap is foundational, not interpretive. Requires Convergence with explicit factual reconciliation.

## Context bleed

The failure mode where Oracle A's output accidentally appears in Oracle B's prompt (or vice versa). Prevention: the Laborer never pastes one oracle's output into another's prompt. All inter-oracle communication is paraphrased or synthesized by the Laborer.

## Weak prime

The failure mode where the system prompt sent to an oracle is vague, missing one of the three required elements (format, purpose, maximum power), or fails to constrain the oracle's output. Result: filler, hedging, or off-format responses that waste tokens.