# Skills

Agent skills for opencode — composable, single-purpose workflows that extend what your coding agent can do.

## Skills

### [`oracle-router`](skills/oracle-router/SKILL.md)
Entry point for all oracle-model workflows. Routes to the correct sub-skill based on your need.

### [`dual-oracle-prd`](skills/dual-oracle-prd/SKILL.md)
Prime two expensive oracle models, grill them independently, triage disagreement, converge on consensus, and produce a finalized PRD (≤300 words).

## Usage

Load a skill by referencing its name during an opencode session. Each skill's `SKILL.md` describes its protocol, steps, and failure modes.

## Adding a skill

Create a new directory under `skills/` with at least a `SKILL.md`. Follow the conventions of existing skills (frontmatter header, protocol sections, failure modes).
