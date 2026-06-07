# /project:prd

## Purpose
Generate or update the product requirements document.

## When to use
Use this command after elicitation artifacts exist and before creating the product backlog.

## Required inputs
- `docs/repo-assessment.md`.
- `docs/elicitation-notes.md`.
- `docs/business-context.md`.
- `docs/risks-assumptions-constraints.md`.

## Allowed reads
- `docs/repo-assessment.md`.
- `docs/elicitation-notes.md`.
- `docs/business-context.md`.
- `docs/risks-assumptions-constraints.md`.

## Allowed writes
- `docs/prd.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not create backlog, prioritization, test, review, or retro files.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create or update `docs/prd.md` with product requirements and CDD expected contracts.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Output template
```markdown
# Product Requirements Document

## Executive Summary
- Summary:

## Problem
- Problem:
- Evidence:

## Goals
- Goal:
- Success metric:

## Non-goals
- Non-goal:
- Reason:

## Users
| User | Need | Context |
|---|---|---|
| TBD | TBD | TBD |

## Scope
### In Scope
- Item:

### Out of Scope
- Item:

## Functional Requirements
- Requirement:
- Acceptance signal:

## Non-functional Requirements
- Requirement:
- Constraint:

## Expected Contracts

### Contract: <name>
- Input:
- Output:
- Rules:
- Expected errors:
- Edge cases:
- Validated by:

## Acceptance Criteria
- Criterion:
- Evidence:

## Risks
- Risk:
- Impact:
- Mitigation:

## Assumptions
- Assumption:

## Open Questions
- Question:

## Backlog Update
- Status:
- Current Phase: Definition
- Last Command: /project:prd
- Next Command: /project:backlog
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/prd.md` exists.
- Executive summary, problem, goals, non-goals, users, scope, functional requirements, non-functional requirements, expected contracts, acceptance criteria, and risks are documented.
- `## Expected Contracts` exists.
- Each contract uses the required `### Contract: <name>` format.
- Missing information is represented as assumptions or open questions.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/project:backlog`
