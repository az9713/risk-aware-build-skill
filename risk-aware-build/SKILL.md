---
name: risk-aware-build
description: Turn uncertain or risky software work into a staged, evidence-backed implementation workflow. Use when the user asks for risk review, unknown discovery, blindspot analysis, implementation planning, agent workflow design, safer coding process, verification planning, handoff preparation, merge readiness, or asks how to convert loose recommendations into durable AI development artifacts.
---

# Risk Aware Build

## Overview

Use this skill to convert uncertainty into durable build artifacts before, during, and after implementation. The core output is a small `ai/` folder that lets one phase feed the next: unknowns, options, tweakable plan, acceptance criteria, implementation notes, verification, handoff, and merge quiz.

## Phase Router

Classify the user's request before acting:

- Pre-implementation: use when the user is deciding what to build, entering an unfamiliar repo/domain, asking for risks, or seeking a plan. Produce or update `ai/01_unknowns.md`, `ai/02_options.md`, `ai/03_tweakable_plan.md`, and `ai/04_acceptance.md` as appropriate.
- During implementation: use when code is being changed and reality diverges from the plan. Maintain `ai/05_implementation_notes.md`.
- Post-implementation: use when validating, preparing review, explaining a change, or deciding whether to merge. Produce or update `ai/06_verification.md`, `ai/07_handoff.md`, and `ai/08_merge_quiz.md`.

If the user only asks for advice, answer directly and name the artifacts you would create. If the user asks to implement, create/update the artifacts in the active repo or workspace.

## Workflow

1. Establish the objective in `ai/00_objective.md` if it is missing or materially unclear.
2. Read the relevant source files, repo areas, specs, tickets, screenshots, logs, or prior artifacts before listing risks.
3. Use `references/workflow-patterns.md` only for the patterns needed by the current phase.
4. Follow `references/artifact-contract.md` for every artifact. Keep evidence, assumptions, and human-sensitive decisions separate.
5. Prefer updating existing `ai/*.md` artifacts over creating duplicates.
6. Do not silently decide human-sensitive items such as data models, migrations, permissions, user-facing behavior, rollout, pricing, compliance, or irreversible deletes. Surface options and ask for sign-off unless the user has already delegated the choice.
7. Before claiming completion, verify with actual evidence: tests, diffs, screenshots, logs, command output, source citations, or explicit limitations.

## Artifact Setup

When creating the workflow folder, copy the templates from `assets/ai-templates/` into an `ai/` folder at the active project root. If an `ai/` folder already exists, update matching files in place and preserve user edits.

Minimum useful first pass:

- `ai/01_unknowns.md`
- `ai/02_options.md`
- `ai/03_tweakable_plan.md`
- `ai/04_acceptance.md`

During implementation, always add:

- `ai/05_implementation_notes.md`

Before handoff, always add:

- `ai/06_verification.md`

Add `ai/07_handoff.md` and `ai/08_merge_quiz.md` when the user needs review, buy-in, or merge readiness.

## Quality Bar

Good output is specific, sourced, and actionable:

- Tie each risk to a source, code path, observed behavior, or stated assumption.
- Rank risks by blast radius and reversibility.
- Convert vague concerns into testable acceptance criteria.
- Put human-sensitive decisions early in the plan.
- Record deviations when implementation reveals reality that the plan missed.
- Treat "no evidence yet" as a status, not as success.

Avoid producing a ceremonial checklist. If a phase does not have enough source basis, say what must be inspected next.
