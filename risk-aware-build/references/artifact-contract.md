# Artifact Contract

Every `ai/*.md` artifact should preserve the same basic contract so later phases can consume it without rediscovering context.

## Required Sections

- Status: draft, awaiting human decision, ready for implementation, in progress, verified, blocked, or superseded.
- Source basis: files, tickets, specs, screenshots, logs, conversations, or links actually inspected.
- Objective: the concrete outcome being pursued.
- Evidence: source-grounded facts, command outputs, screenshots, test results, or explicit absence of evidence.
- Unknowns and risks: grouped by blast radius and reversibility.
- Assumptions: agent-made assumptions that must not be mistaken for confirmed facts.
- Human-sensitive decisions: choices that materially affect product behavior, data models, migrations, security, permissions, rollout, user trust, cost, or irreversible state.
- Next artifact: the next file or phase that should consume this output.

## Risk Shape

Use this shape for each material risk:

```text
Risk:
Why it matters:
Evidence:
Likelihood:
Impact:
Reversibility:
Mitigation:
Acceptance test:
Owner decision needed:
```

## Decision Shape

Use this shape for choices the agent should not silently make:

```text
Decision:
Options:
Tradeoffs:
Recommended default:
Why this is human-sensitive:
Decision needed by:
```

## Evidence Rules

- Prefer direct source evidence over generic best practice.
- If a risk is inferred, label it as inference and name the basis.
- If evidence is missing, write `Evidence needed:` rather than pretending certainty.
- Verification artifacts must include actual commands, screenshots, diffs, logs, or test names when available.
