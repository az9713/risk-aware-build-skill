# Workflow Patterns

Use only the patterns needed for the current phase. These patterns are prompts and artifact shapes, not separate mandatory steps.

## 1. Blindspot Pass

Use when: entering an unfamiliar repo, subsystem, domain, or high-risk change.
Inputs: objective, relevant files, prior plans, known constraints.
Output artifact: `ai/01_unknowns.md`.
Prompt shape: find unknown unknowns that could make the request wrong, incomplete, dangerous, or expensive; explain where each appears and rewrite the implementation prompt with blindspots included.
Failure mode to avoid: generic risk lists not tied to code, sources, or domain facts.

## 2. Teach Me My Unknowns

Use when: the user lacks domain vocabulary or quality criteria.
Inputs: task, domain, examples of desired outcome if available.
Output artifact: `ai/01_unknowns.md` or a domain explainer section inside it.
Prompt shape: teach the mental model, vocabulary ladder, what good looks like, common mistakes, and prompts the user could not have written before.
Failure mode to avoid: over-teaching basics while missing the specific decision criteria needed for this build.

## 3. Four Design Directions

Use when: taste, layout, product shape, or information hierarchy is under-specified.
Inputs: data, audience, constraints, comparable surfaces.
Output artifact: `ai/02_options.md`; optionally a throwaway HTML mock.
Prompt shape: produce four intentionally different directions with what to steal, what to skip, and tradeoffs.
Failure mode to avoid: four cosmetic variants of the same idea.

## 4. Mock Before Wiring

Use when: a UI or workflow should be reacted to before real integration.
Inputs: feature goal, fake data, existing UI conventions.
Output artifact: `ai/02_options.md` plus mock artifact path.
Prompt shape: build disposable mockups and identify product questions before touching real app code.
Failure mode to avoid: implementing backend or persistent state before layout and flow are settled.

## 5. Brainstorm Intervention Space

Use when: the initial solution may be too narrow.
Inputs: problem statement, code/product surface, user behavior evidence.
Output artifact: `ai/02_options.md`.
Prompt shape: list possible interventions from cheapest to most ambitious, with evidence, effort, impact, and what would make each worth doing.
Failure mode to avoid: committing to the first plausible solution.

## 6. Interview Me One Question At A Time

Use when: requirements are ambiguous and answers would change architecture, data, permissions, UX, or sequencing.
Inputs: objective, current unknowns, constraints.
Output artifact: `ai/03_tweakable_plan.md` decision table.
Prompt shape: ask one high-leverage question at a time and update a decisions table after each answer.
Failure mode to avoid: asking many low-value questions or using interview mode to delay obvious source inspection.

## 7. Reference Port

Use when: desired behavior exists in another codebase, library, screenshot, or product.
Inputs: reference path or URL, target environment.
Output artifact: `ai/03_tweakable_plan.md` semantics map.
Prompt shape: map preserved behavior, intentional changes, dropped details, edge cases, and test vectors before implementation.
Failure mode to avoid: copying implementation details without preserving semantics.

## 8. Tweakable Implementation Plan

Use when: preparing to implement.
Inputs: objective, unknowns, options, decisions, constraints.
Output artifact: `ai/03_tweakable_plan.md`.
Prompt shape: sort plan by likelihood the human will tweak the decision, with data model, type/interface, UX, permissions, migrations, rollout, and rollback before mechanical work.
Failure mode to avoid: chronological plans that bury high-impact decisions.

## 9. Implementation Notes

Use when: code is being changed.
Inputs: approved plan and acceptance criteria.
Output artifact: `ai/05_implementation_notes.md`.
Prompt shape: log discoveries, deviations, conservative choices, human-judgment items, and lessons for the next prompt.
Failure mode to avoid: silently drifting from the plan.

## 10. Buy-In / Pitch Doc

Use when: reviewers, PMs, security, design, leadership, or operators need to understand or approve the work.
Inputs: prototype, spec, implementation notes, verification evidence, rollout plan.
Output artifact: `ai/07_handoff.md`.
Prompt shape: lead with the demo or outcome, then explain what changed, evidence, objections, risks, rollback, sign-offs, and exact ask.
Failure mode to avoid: long narrative that hides the review decision.

## 11. Merge-Readiness Quiz

Use when: the user needs to understand a change before merge or production release.
Inputs: diff, implementation notes, tests, operational context.
Output artifact: `ai/08_merge_quiz.md`.
Prompt shape: explain mental model, non-obvious behaviors, dependencies, risks, and quiz the human on merge-critical understanding.
Failure mode to avoid: trivia questions instead of questions that reveal operational misunderstanding.
