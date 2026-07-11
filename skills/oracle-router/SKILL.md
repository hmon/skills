---
name: oracle-router
description: Router for all oracle-related skills. Lists each skill and when to reach for it. User-invoked only.
disable-model-invocation: true
---

The **oracle-router** is the single entry point for all oracle-model workflows. When you need to consult expensive external models, type this skill's name and it will route you to the correct sub-skill.

---

## Skills

### dual-oracle-prd

**When to reach for it:** You need to ask a focused technical question to two expensive oracle models, resolve any disagreement between them, and produce a single finalized Product Requirements Document.

**Trigger phrases:** "run the oracles", "dual oracle", "oracle PRD", "grill the oracles", "get oracle consensus", "expensive model review", "two-oracle decision"

**What it does:**
1. Primes both Oracle A and Oracle B with a system prompt (format, purpose, maximum power).
2. Grills both with the same brief context + question.
3. Triage: classifies answers as Aligned, Conflict, or Drift.
4. Converge: if not Aligned, sends a secondary disagreement prompt (up to 3 rounds).
5. Drafts a concise PRD (≤300 words).
6. Reviews the PRD with both oracles.
7. Finalizes into a single PRD file.

**Cost profile:** High — two oracles, multiple rounds possible. Use only for decisions that need consensus.

---

## How to use

Type `oracle-router` to see this list. Then type the name of the skill you want to run (e.g., `dual-oracle-prd`) to invoke it directly.

---

## Adding new skills

When you create a new oracle workflow, add it to this router under a new heading. Include: when to reach for it, trigger phrases, what it does, and cost profile.