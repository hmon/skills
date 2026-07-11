---
name: dual-oracle-prd
description: Prime two expensive oracle models, grill them, resolve disagreements, and finalize a single PRD. User-invoked only.
disable-model-invocation: true
---

A skill for the **Laborer** — a coding agent with local code access — to **prime** two expensive **oracle** models, **grill** them with focused questions, **triage** their answers, **converge** on agreement, and produce a single finalized **PRD**.

---

## Leading words

- **Prime** — produce a system prompt for the user to relay to both oracles, dictating response format, purpose, and power level before any question is asked.
- **Grill** — produce a brief context + one focused question for the user to relay to each oracle independently; expect a terse, high-signal answer back via the user.
- **Triage** — compare oracle responses (relayed by the user) to decide if they agree, disagree, or speak past each other.
- **Converge** — when oracles disagree, craft a secondary prompt (surfacing the disagreement) for the user to relay to both oracles, forcing a decision.
- **PRD** — the concise product-requirements document that captures the final, agreed-upon decision.
- **Laborer** — the agent running this skill; produces prompts for the user to relay to oracles.
- **Oracle** — an expensive external model; brief responses only.

---

## Communications channel

The Laborer (this agent) **cannot directly invoke** oracle models. Instead:

1. The Laborer produces a **prompt block** — the exact text the user should copy and paste to the oracle.
2. The user sends the prompt to the oracle(s), copies the response(s) back to the chat.
3. The Laborer reads the responses, triages them, and produces the next prompt.
4. Repeat until convergence — the user is the human router for all oracle messages.

The user must keep oracle responses strictly separated (no forwarding one oracle's answer to the other).

## Condition: Clean relay format

When the Laborer produces a prompt for an oracle, the **entire message** to the user must be **only** the copy-paste text — no explanatory preamble, no markdown framing, no commentary. The user should be able to select the entire response and paste it directly to the oracle without editing. If the Laborer needs to communicate anything else (triaging, next steps), it must be placed in a separate message after the relay prompt. Each oracle round produces exactly one relay message; any analysis happens in subsequent messages.

---

## The protocol

Oracles never communicate with each other. The Laborer produces prompts; the user relays them. No chat history is forwarded between oracles.

---

## Steps

### 1. Prime the oracles

**Completion criterion:** Both oracles have acknowledged their system prompt.

Before the first grill, generate a **prime prompt** and ask the user to relay it identically to both Oracle A and Oracle B independently. The prime prompt must:

- Dictate their **response format**: conclusion first, then reasoning in ≤2 sentences; no filler, no hedging.
- State their **purpose**: they are an Oracle answering technical questions with maximum depth and precision.
- Instruct them to **use their maximum power**: no holding back, no conservative estimates.

Example prime prompt (adapt to the domain):

&gt; You are an Oracle — a high-cost reasoning model. Your purpose is to answer technical questions with maximum depth and precision. You must be terse: every sentence must carry signal. No hedging, no filler. Use your maximum power. Format: state your conclusion first, then your reasoning in ≤2 sentences. If uncertain, say so in one word.

Wait for the user to confirm both oracles acknowledged.

### 2. Grill both oracles

**Completion criterion:** Both oracle responses have been relayed back by the user.

Produce a prompt with brief context (≤3 sentences) + one focused question. Ask the user to relay it to Oracle A and Oracle B independently. Label each clearly. Remind the user not to show one oracle's answer to the other.

Context must be ≤ 3 sentences + the question. Oracles are expensive; brevity is the constraint.

### 3. Triage

**Completion criterion:** The Laborer has classified the pair of answers into one of three buckets.

Read both answers side-by-side. Classify:

- **Aligned** — same conclusion, same reasoning.
- **Conflict** — same facts, different conclusion.
- **Drift** — different facts, different assumptions, or talking past each other.

If **Aligned**, jump to Step 5 (Draft PRD).

If **Conflict** or **Drift**, continue to Step 4.

### 4. Converge

**Completion criterion:** Both oracles have responded to the secondary prompt (relayed by the user) and the Laborer can classify the new pair as Aligned.

Write one secondary prompt that:

- States the exact point of disagreement (quote both oracles).
- Asks each oracle to defend their position in ≤ 2 sentences.
- Asks each oracle to identify what would change their mind.
- Reminds the oracle: "You cannot see the other oracle's answer."

Ask the user to relay this secondary prompt to both oracles independently. Return to Step 3 (Triage) with the new answers. Repeat **Converge** until the pair is Aligned, up to a maximum of 3 rounds. If still not Aligned after 3 rounds, note the deadlock in the PRD and pick the answer with the stronger reasoning.

### 5. Draft PRD

**Completion criterion:** A concise PRD exists that captures the agreed decision, the reasoning, and any deadlocks.

Write a PRD with these sections:

1. **Decision** — one sentence, the final answer.
2. **Reasoning** — 2–3 sentences, synthesizing the oracle consensus.
3. **Scope** — what the decision covers and what it excludes.
4. **Deadlocks** — any points where oracles remained split after 3 rounds (omit if none).

Keep the PRD under 300 words.

### 6. Review

**Completion criterion:** Both oracles have reviewed the PRD (responses relayed by the user) and either approved it or proposed edits.

Ask the user to relay the drafted PRD to Oracle A and Oracle B independently with this prompt:

&gt; "Review this PRD. Approve or propose one concrete edit. Keep your response to 2 sentences."

Remind the user not to forward one oracle's review to the other.

### 7. Finalize

**Completion criterion:** A single PRD file is written and both oracles have seen the final version.

If both oracles approved without edits, the draft is final.

If an oracle proposed edits, apply the edit if both oracles proposed the same change; if they proposed different changes, use your judgment as Laborer to reconcile or escalate to the user.

Write the final PRD to a single file. Ask the user to relay the final version to both oracles for a final acknowledgment.

---

## Failure modes

- **Premature completion** — ending a grill before the oracle has actually answered the question. Defence: re-read the oracle's response; if it doesn't directly answer the question, re-prompt.
- **Context bleed** — accidentally forwarding Oracle A's answer to Oracle B. Defence: remind the user never to paste one oracle's output into another's prompt.
- **Sediment** — letting the PRD grow with every round of convergence. Defence: the 300-word cap is hard; cut, don't expand.
- **Drift blindness** — misclassifying Drift as Conflict. Defence: if the oracles use different facts, it's Drift; surface the factual gap in the secondary prompt.
- **Weak prime** — producing a vague system prompt that doesn't constrain the oracle's output. Defence: include all three elements (format, purpose, maximum power) in every prime prompt.
- **Orphan prompt** — producing a prompt but the user has no oracle available. Defence: before invoking this skill, confirm the user has access to two independent oracle models (e.g., Claude, GPT-4o, Gemini) and is willing to relay prompts.