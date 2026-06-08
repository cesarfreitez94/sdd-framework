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
- Do not invent missing information. If implementation-critical information is missing, write assumptions and blocking open questions.
- Do not update files outside the allowed writes.

## Expected output
Create or update `docs/prd.md` with product requirements and CDD expected contracts.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/prd.md`
- Runtime output: `./docs/prd.md` in the target repository

## Critical Decision Closure Rule
For implementation-critical topics, the PRD must either:
- Close the decision as a requirement.
- Close the decision as a contract.
- Close the decision as an acceptance criterion.
- Declare the decision out of scope.
- Mark the decision as a blocking open question and route back to `/project:elicit`.

Implementation-critical topics include, when applicable:
- File mapping.
- Overwrite policy.
- Default behavior.
- Path resolution.
- Protected files.
- Dry-run.
- Backup.
- Force.
- Yes/confirmation.
- Install/update behavior.
- Validation environments.
- Greenfield/brownfield validation.
- External dependencies.
- Workflow engine exclusion.
- Session persistence exclusion.
- Coordinator agent exclusion.

Do not leave important implementation-critical decisions as vague open questions when the source artifacts already contain enough context to decide them.

Every important elicitation point, risk, assumption, or open question must be converted into one of:
- Functional requirement.
- Non-functional requirement.
- Expected contract.
- Acceptance criterion.
- Risk.
- Explicit non-goal.
- Blocking open question.

Use the source artifacts to close decisions whenever the evidence is sufficient. If a decision cannot be closed because the source artifacts are insufficient, mark it under `## Blocking Open Questions` and set the recommended next command to `/project:elicit`, not `/project:backlog`.

## Backlog Readiness Gate
The PRD must explicitly declare one of:

```markdown
## Backlog Readiness
- Status: READY | NOT_READY
- Reason:
- Next command:
```

Rules:
- If the PRD contains implementation-critical open questions, status must be `NOT_READY`.
- If status is `NOT_READY`, the next command must be `/project:elicit`.
- If status is `READY`, the next command may be `/project:backlog`.
- The command must not recommend `/project:backlog` while `## Blocking Open Questions` contains unresolved critical decisions.

## Required PRD Specificity
The generated PRD must be specific enough to generate a backlog without requiring major inference.

For installer or CLI products, explicitly define the following when applicable:
- Source/destination file mapping.
- Command behavior.
- Path handling.
- Overwrite behavior.
- Default behavior.
- Force behavior.
- Backup behavior.
- Dry-run behavior.
- Confirmation/yes behavior.
- README handling.
- Validation environments.
- Greenfield/brownfield validation cases.
- Explicit exclusions.

For other product types, infer equivalent specificity dimensions from the elicitation artifacts and make them explicit in requirements, contracts, acceptance criteria, risks, or non-goals.

## Reviewer Warning
If the command cannot close a decision because the source artifacts are insufficient, it must mark the question as `## Blocking Open Questions` and set the recommended next command back to `/project:elicit`, not `/project:backlog`.

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

### FR-XXX — <name>
- Description:
- Source:
- Behavior:
- Edge cases:
- Acceptance criteria:

## Non-functional Requirements
- Requirement:
- Constraint:

## Expected Contracts

### Contract: <name>
- Source:
- Input:
- Output:
- Rules:
- Expected errors:
- Edge cases:
- Validation method:
- Related acceptance criteria:

## Acceptance Criteria

### AC-XXX — <name>
- Given:
- When:
- Then:
- Evidence:

## Risks
- Risk:
- Impact:
- Mitigation:

## Assumptions
- Assumption:

## Backlog Readiness
- Status: READY | NOT_READY
- Reason:
- Next command: /project:backlog or /project:elicit

## Blocking Open Questions
- Question:
- Why it blocks backlog:
- Needed decision:

## Backlog Update
- Status:
- Current Phase: Definition
- Last Command: /project:prd
- Next Command: /project:backlog only when `## Backlog Readiness` is `READY`; otherwise /project:elicit.
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/prd.md` exists.
- Executive summary, problem, goals, non-goals, users, scope, functional requirements, non-functional requirements, expected contracts, acceptance criteria, and risks are documented.
- `## Backlog Readiness` exists.
- `Status: READY` is used only when the PRD can feed `/project:backlog` without major inference.
- `Status: NOT_READY` routes back to `/project:elicit`.
- `## Expected Contracts` exists.
- Each contract uses the required `### Contract: <name>` format.
- No critical behavior remains as a vague open question.
- Critical unresolved decisions are not hidden under generic open questions.
- All key risks and assumptions are either converted to requirements, contracts, acceptance criteria, or marked as blocking.
- The PRD can feed `/project:backlog` without major inference.
- Safety behavior is testable when the product modifies user files.
- Scope exclusions are explicit.
- Each functional requirement uses the required `### FR-XXX — <name>` format and includes description, source, behavior, edge cases, and acceptance criteria.
- Each contract includes source, input, output, rules, expected errors, edge cases, validation method, and related acceptance criteria.
- Each acceptance criterion uses the required `### AC-XXX — <name>` format and is directly testable with given, when, then, and evidence.
- Missing implementation-critical information is represented as a blocking open question, not a vague non-blocking question.
- If any decision cannot be closed because source artifacts are insufficient, `## Blocking Open Questions` exists, `## Backlog Readiness` is `NOT_READY`, and the next command is `/project:elicit`.
- The command does not recommend `/project:backlog` while `## Blocking Open Questions` contains unresolved critical decisions.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
Use the command declared by `## Backlog Readiness`: `/project:backlog` only when status is `READY`; otherwise `/project:elicit`.
