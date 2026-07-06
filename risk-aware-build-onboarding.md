# Risk-Aware Build Skill Onboarding

## What This Skill Is

`risk-aware-build` is a Codex skill for turning uncertain software work into a staged, evidence-backed development workflow.

Use it when a task has meaningful ambiguity, hidden constraints, implementation risk, unclear requirements, or review/merge consequences. The skill helps Codex slow down at the right moments: discover unknowns before coding, expose human-sensitive decisions, maintain notes while implementation reality diverges from the plan, and verify the final result with concrete evidence.

The skill is not a general project-management checklist. Its purpose is to make AI-assisted development safer by preserving the reasoning trail across durable artifacts.

The central output is an `ai/` folder in the active project:

```text
ai/
  00_objective.md
  01_unknowns.md
  02_options.md
  03_tweakable_plan.md
  04_acceptance.md
  05_implementation_notes.md
  06_verification.md
  07_handoff.md
  08_merge_quiz.md
```

These files are the stitching layer. Each phase feeds the next, so the agent does not rediscover the same context or silently drift from the plan.

## What Problem It Solves

Strong coding agents often fail less because they cannot write code and more because they make plausible assumptions in places where the user, product, repo, or domain had hidden constraints.

`risk-aware-build` is designed for that failure mode. It asks Codex to separate:

- what is known from source evidence
- what is inferred
- what is still unknown
- what the agent can decide safely
- what requires human sign-off
- what must be verified before claiming success

The skill is especially useful when the cost of a wrong assumption is high.

## When To Use It

Use `risk-aware-build` when any of these are true:

- You are entering an unfamiliar repo, subsystem, framework, or domain.
- The request sounds simple but may touch data models, permissions, migrations, security, billing, user trust, or rollout.
- You want the agent to discover blindspots before implementation.
- You need multiple options before committing to a solution.
- You want human-sensitive decisions surfaced early instead of buried in implementation details.
- You expect a long implementation where reality may diverge from the plan.
- You need a verification trail for review, merge, or production deployment.
- You want a handoff doc, reviewer pitch, or merge-readiness quiz.
- You are converting loose risk suggestions into a reusable AI development process.

## When Not To Use It

Do not use it for every tiny task. It is probably unnecessary for:

- one-line copy edits
- trivial formatting changes
- simple renames with no behavioral impact
- isolated test fixture updates
- small mechanical refactors where the source basis is obvious
- tasks where the user explicitly asks Codex to skip planning and make a narrow change

The skill adds value when uncertainty matters. For low-risk work, it can become ceremony.

## How To Trigger It

The most direct trigger is to name the skill:

```text
Use $risk-aware-build to plan this feature before implementation.
```

Other natural triggers should also work because the skill description covers risk review, unknown discovery, planning, safer implementation, verification, and merge readiness.

Good trigger phrases:

```text
Use $risk-aware-build to do a blindspot pass before we touch the code.
```

```text
Use $risk-aware-build to turn this vague feature idea into an implementation plan.
```

```text
Before coding, use $risk-aware-build to find unknown unknowns in this repo.
```

```text
Use $risk-aware-build to create an ai/ plan for this migration.
```

```text
Use $risk-aware-build during implementation and keep implementation notes.
```

```text
Use $risk-aware-build to verify this change and prepare a merge-readiness quiz.
```

```text
Use $risk-aware-build to package this prototype into a reviewer handoff doc.
```

Implicit trigger phrases:

```text
What are the risks before we implement this?
```

```text
Find the blindspots in this plan.
```

```text
I do not know what I do not know about this subsystem.
```

```text
Give me options before we commit to a solution.
```

```text
Build this, but log deviations as you discover them.
```

```text
Before I merge, make sure I understand the change.
```

## How The Workflow Works

### 1. Pre-Implementation

Use this phase before code changes begin.

Codex should inspect relevant source files, specs, tickets, screenshots, logs, or prior artifacts, then produce some or all of:

```text
ai/00_objective.md
ai/01_unknowns.md
ai/02_options.md
ai/03_tweakable_plan.md
ai/04_acceptance.md
```

This phase is for surfacing hidden constraints and shaping the implementation prompt. It should put human-sensitive decisions first: data model choices, UX behavior, permissions, migrations, rollout, rollback, and other choices that materially affect the product.

Example request:

```text
Use $risk-aware-build on this repo. I want to add SSO, but I do not understand the auth module. Do a blindspot pass, propose options, and write a tweakable implementation plan. Do not implement yet.
```

### 2. During Implementation

Use this phase once coding begins.

Codex should maintain:

```text
ai/05_implementation_notes.md
```

This file records discoveries, deviations from the plan, conservative choices, open questions, and lessons for the next attempt.

Example request:

```text
Now implement the approved plan. Use $risk-aware-build and keep ai/05_implementation_notes.md updated whenever the codebase forces a deviation.
```

### 3. Post-Implementation

Use this phase after code changes are made.

Codex should produce or update:

```text
ai/06_verification.md
ai/07_handoff.md
ai/08_merge_quiz.md
```

This phase is for evidence and transfer of understanding. Codex should record commands run, tests passed or skipped, screenshots, logs, diffs, residual risks, reviewer objections, rollback notes, and merge-critical questions.

Example request:

```text
Use $risk-aware-build to verify the change, write a handoff doc for reviewers, and quiz me before merge.
```

## Project Scenarios That Benefit Most

### New Feature In An Unfamiliar Codebase

Use the skill when you know the user-facing goal but do not understand the existing architecture.

Example:

```text
Use $risk-aware-build to plan adding workspace-level invitations to this app. I do not know the account model well enough, so start with unknowns and options.
```

Why it helps: it forces Codex to inspect the repo, identify hidden account/permission constraints, and avoid inventing a data model too early.

### Authentication, Authorization, And Permissions

Use the skill for auth providers, roles, access control, session handling, secrets, tenant boundaries, and admin tools.

Example:

```text
Use $risk-aware-build before changing role permissions. I want a risk pass, acceptance tests, and merge-readiness quiz.
```

Why it helps: permission changes often fail through edge cases, not syntax errors.

### Data Migrations And Schema Changes

Use the skill when a change touches persistent data, migrations, backfills, analytics definitions, or rollback paths.

Example:

```text
Use $risk-aware-build to plan this billing schema migration. Put migration and rollback decisions before mechanical implementation steps.
```

Why it helps: the skill separates reversible implementation work from irreversible data decisions.

### UI Or Product Workflow Changes

Use the skill when taste, information hierarchy, or workflow behavior is unclear.

Example:

```text
Use $risk-aware-build to explore four different designs for this dashboard before wiring real data.
```

Why it helps: it supports mock-before-wiring and option-space exploration, avoiding expensive implementation churn.

### Long-Running Agentic Coding Tasks

Use the skill when a task may take many steps and Codex may discover surprises along the way.

Example:

```text
Use $risk-aware-build for this refactor. Keep implementation notes and log any deviation from the plan.
```

Why it helps: the `implementation_notes` artifact prevents silent drift and captures what the next run should know.

### Security, Privacy, Compliance, Or Trust-Sensitive Work

Use the skill when mistakes could expose data, weaken controls, or confuse users about safety-critical behavior.

Example:

```text
Use $risk-aware-build to review the risks in this export feature. I need evidence, assumptions, and human-sensitive decisions separated.
```

Why it helps: the skill requires source-grounded risk claims and explicit verification.

### Reviewer Handoff And Merge Readiness

Use the skill when another person needs to understand, approve, or operate the change.

Example:

```text
Use $risk-aware-build to write a reviewer handoff and merge-readiness quiz for this PR.
```

Why it helps: it turns implementation work into reviewable evidence and checks whether the user understands the change before merge.

## What Good Output Looks Like

Good `risk-aware-build` output is specific and evidence-backed.

It should include:

- file paths, code references, command outputs, screenshots, logs, or cited source facts
- risks ranked by blast radius and reversibility
- assumptions clearly labeled as assumptions
- human decisions separated from agent decisions
- acceptance criteria that can be tested
- verification evidence before completion claims
- residual risk and unverified areas

Weak output looks like:

- generic best-practice lists
- risks with no source basis
- plans sorted only by execution order
- hidden data, security, UX, or rollout decisions
- claims of completion without tests or evidence
- handoff docs that summarize work but do not state the exact review ask

## Example End-To-End Prompt

```text
Use $risk-aware-build for this task.

Goal: add organization-level API keys to this repo.

Start with pre-implementation only. Read the relevant auth, billing, and settings code. Create or update ai/00_objective.md, ai/01_unknowns.md, ai/02_options.md, ai/03_tweakable_plan.md, and ai/04_acceptance.md.

Do not implement yet. Put human-sensitive decisions first, especially data model, permissions, key visibility, revocation behavior, audit logging, migration, and rollback.
```

Follow-up after approval:

```text
Proceed with implementation using the approved plan. Keep ai/05_implementation_notes.md updated. If the codebase forces a deviation, choose the conservative path, log it, and continue unless the decision is irreversible or product-defining.
```

Final verification prompt:

```text
Use $risk-aware-build to complete ai/06_verification.md, ai/07_handoff.md, and ai/08_merge_quiz.md. Include actual commands run, test results, residual risks, and questions I must answer before merge.
```

## Practical Adoption Pattern

Start by using the skill only on high-uncertainty tasks. Do not force it onto every small change.

A good adoption ladder:

1. Use it for planning only.
2. Add implementation notes for long coding tasks.
3. Add verification artifacts before completion.
4. Add handoff and merge quiz for review-heavy changes.
5. After repeated use, split out specialized subskills only if a phase becomes independently valuable.

The first version is intentionally one orchestrator skill. The clean split is not eleven separate skills; it is one workflow skill, a shared artifact contract, and templates that preserve continuity across phases.
