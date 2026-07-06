# Risk-Aware Build Development Journey

## Starting Point: Thariq's X Post

This project started from Thariq's X post, "A Field Guide to Fable: Finding Your Unknowns." The post's central idea is that strong agentic coding models change the bottleneck. The hard part is no longer only whether the model can write code. The hard part is whether the human and agent can discover the unknowns that would make the work wrong, incomplete, expensive, or misaligned.

Thariq frames the problem as a gap between the map and the territory. The map is what we give the agent: prompts, skills, context, project notes, and instructions. The territory is the real working environment: the codebase, user intent, constraints, prior decisions, edge cases, product expectations, and organizational reality. The gap between those two is where the agent has to guess.

The post breaks unknowns into a useful vocabulary:

- Known knowns: what is already in the prompt.
- Known unknowns: questions we know we have not answered.
- Unknown knowns: things we would recognize if we saw them, but would not think to write down.
- Unknown unknowns: issues, quality criteria, constraints, or domain facts we have not considered at all.

The practical lesson was that agentic coding quality depends on shrinking that gap. Thariq's post offered a collection of patterns for doing that: blindspot passes, brainstorming, prototypes, interviews, references, implementation plans, implementation notes, pitch docs, and quizzes.

The first temptation was to treat those patterns as a list of discrete skills. There are eleven recognizable workflows, so it is natural to ask whether there should be eleven Codex skills.

That turned out to be the wrong first abstraction.

## The Initial Question

The question was not simply: "Can these patterns become skills?"

The better question was: "What makes these patterns useful together?"

The answer is continuity. A blindspot pass is useful because it informs options. Options are useful because they shape a tweakable plan. A plan is useful because implementation notes can record when reality diverges from it. Implementation notes are useful because verification and handoff can explain what actually happened. A merge quiz is useful because it checks whether the human understands the final change well enough to ship it.

The power is not in any one pattern. The power is in the chain.

That chain needs a durable handoff layer. Without that layer, each pattern becomes another prompt trick. The agent can produce a nice risk list, but the risks may never reach the plan. The agent can write a plan, but implementation may quietly drift. The agent can verify, but the verification may not connect back to the original acceptance criteria.

So the design moved away from "eleven individual skills" and toward an operating-system-style workflow tool.

## Why Not Eleven Individual Skills

Splitting the patterns into eleven skills would create several risks.

First, the boundaries are blurry. Blindspot passes, teaching unknowns, brainstorming options, and interviews all overlap. They are different moves in the same pre-implementation discovery phase. If each became a separate skill, Codex would have to choose among several similar triggers, and it might choose wrong or skip the transition between them.

Second, the important state lives between phases. A standalone `blindspot-pass` skill might produce a good report, but unless the next phase knows how to consume it, the value decays. The same is true for implementation notes, verification logs, and handoff docs. The product is not the document; the product is the preserved reasoning trail.

Third, many of the patterns are not full capabilities. They are tactical moves. A skill should usually represent a coherent job Codex can perform. "Mock before wiring" is useful, but it is not always a complete workflow. It is one move inside a larger risk-aware build process.

Fourth, too many similar skills increase routing noise. Skill descriptions are used for triggering. If many skills mention unknowns, risk, planning, implementation, and verification, the model may not reliably know which one to load. That creates the opposite of the intended result: more hidden assumptions.

Fifth, splitting too early would freeze the wrong architecture. The real usage patterns are not yet known. Version 1 should preserve flexibility until repeated use reveals which phases deserve independent tools.

## The Operating-System Recommendation

The recommendation was to build one orchestrator skill first: `risk-aware-build`.

The phrase "operating system" here does not mean a large software platform. It means a small coordination layer that standardizes how agentic development work moves from uncertainty to implementation to verification.

The skill's job is to answer:

- What phase are we in?
- What should be inspected before acting?
- Which artifact should be created or updated?
- Which decisions are safe for the agent to make?
- Which decisions require human sign-off?
- What evidence is required before claiming completion?
- What context should the next phase inherit?

That is why the first version is one skill with a shared artifact contract, references, and templates.

The durable artifact sequence is:

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

These files are the workflow's memory. They make the risk process inspectable and reusable across turns, sessions, and agents.

## What We Built In Version 1

Version 1 is intentionally small.

It contains:

```text
risk-aware-build/
  SKILL.md
  agents/openai.yaml
  references/
    artifact-contract.md
    workflow-patterns.md
  assets/
    ai-templates/
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

The skill itself is a router. It classifies the request into one of three phases:

- Pre-implementation: unknowns, options, tweakable plan, acceptance criteria.
- During implementation: implementation notes and deviation tracking.
- Post-implementation: verification, handoff, merge-readiness quiz.

The references hold the reusable concepts:

- `workflow-patterns.md` summarizes the eleven patterns from the Thariq-inspired workflow.
- `artifact-contract.md` defines the common structure every phase artifact should preserve.

The templates are deliberately plain. They are not polished reports. They are working documents for an agentic development process.

## Why The Name Changed

The early placeholder name was centered on unknowns. That was accurate but too narrow and abstract. It described the diagnostic move, not the practical outcome.

The final name, `risk-aware-build`, is clearer because it says what the skill is for: building software while staying aware of the risks created by uncertainty.

The skill still uses unknown discovery, but it does not stop there. It turns unknowns into options, options into plans, plans into implementation notes, notes into verification, and verification into handoff.

## Design Principles Behind Version 1

### 1. Optimize For Continuity

The primary design goal is stitching. Each artifact should make the next phase easier and safer. The system should avoid one-off reports that do not affect implementation.

### 2. Keep The Skill Lean

The skill should not carry every detail in `SKILL.md`. The body stays short enough to load quickly and reliably. Detailed patterns and templates live in references and assets.

### 3. Preserve Human Judgment

The skill is designed to stop agents from silently deciding things that humans should decide: data models, permissions, migrations, rollout behavior, user-facing semantics, security tradeoffs, and irreversible operations.

### 4. Require Evidence

A risk-aware workflow is only useful if claims are grounded. The skill asks Codex to tie risks to inspected files, source facts, command output, screenshots, logs, tests, or explicit assumptions.

### 5. Avoid Premature Skill Splitting

Version 1 is an orchestrator because the boundaries between subskills are still unproven. Splitting should happen later, after repeated use shows where a phase has a stable input, output, trigger, and validation method.

## What Version 1 Is Not

Version 1 is not a full plugin.

A plugin may eventually be useful if we want to bundle multiple skills, scripts, hooks, MCP configuration, dashboards, or installable workflow assets. But the first implementation did not need that complexity. It needed a usable skill and a durable artifact protocol.

Version 1 is also not a full risk-management framework. It does not try to model every kind of engineering risk. It provides a practical harness for agentic coding tasks where hidden assumptions matter.

Version 1 is not meant for every small edit. It is useful when uncertainty, review cost, implementation drift, or production consequences justify the extra structure.

## Future Work

The next step is to use the skill on real tasks and watch where it helps or gets in the way.

Possible future improvements:

1. Add a setup script that copies `assets/ai-templates/` into a project's `ai/` folder without manual file handling.
2. Add a validation script that checks whether each `ai/*.md` artifact contains source basis, evidence, assumptions, human-sensitive decisions, and next-step links.
3. Create example filled-out artifacts from a real repo task so future agents can see the expected quality bar.
4. Add domain-specific reference files for common high-risk areas: auth, permissions, migrations, billing, security, privacy, UI workflows, and deployment.
5. Split out subskills only after usage proves the boundaries. Likely candidates are `risk-aware-plan`, `implementation-notes`, and `merge-readiness`.
6. Package the skill into a plugin if the workflow grows to include scripts, hooks, dashboards, or multiple cooperating skills.
7. Forward-test the skill with fresh agents on realistic tasks to see whether the instructions generalize without hidden context.
8. Add a lightweight scoring rubric for risk artifacts: source-grounding, decision clarity, reversibility, verification quality, and handoff usefulness.

## Version 1 Success Criteria

Version 1 succeeds if it changes agent behavior in a few concrete ways:

- Codex inspects source context before listing risks.
- Unknowns become implementation-relevant decisions, not generic warnings.
- Plans surface human-sensitive choices early.
- Implementation notes capture reality when it diverges from the plan.
- Verification records actual evidence instead of relying on confidence.
- Handoff artifacts help reviewers understand what changed and what still needs judgment.

The goal is not to make the workflow heavier. The goal is to make the right parts of AI-assisted development explicit before they become expensive.

## Bottom Line

Thariq's post supplied the core insight: powerful models make unknowns more important, not less. The better the model, the more work it can do while making assumptions. That increases the value of discovering and managing those assumptions.

Our design response was to turn the insight into a small operating-system-style skill: one router, one artifact contract, one sequence of durable files, and a set of workflow patterns that can be used as needed.

`risk-aware-build` is version 1 of that idea. It is intentionally conservative. It keeps the workflow together first. Only after real use should it be split into more specialized skills or expanded into a plugin.
