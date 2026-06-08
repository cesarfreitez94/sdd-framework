# /project:elicit

## Purpose
Capture business and functional context using lightweight BABOK-style elicitation.

## When to use
Use this command after `/project:discover-repo` and before creating or updating the PRD.

## Required inputs
- `docs/repo-assessment.md`.
- Existing business, product, or technical docs if present.
- User-provided business context when available.

## Allowed reads
- `docs/repo-assessment.md`.
- Existing docs if present.

## Allowed writes
- `docs/elicitation-notes.md`.
- `docs/business-context.md`.
- `docs/risks-assumptions-constraints.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not create or modify PRD, backlog, prioritization, test, review, or retro files.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create elicitation notes, business context, and risks/assumptions/constraints documentation that can feed the PRD.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/elicitation-notes.md`, `.opencode/templates/docs/business-context.md`, `.opencode/templates/docs/risks-assumptions-constraints.md`
- Runtime output: `./docs/elicitation-notes.md`, `./docs/business-context.md`, `./docs/risks-assumptions-constraints.md` in the target repository

## Critical Decision Capture Rule
When the product involves file generation, installation, migration, automation, CLI tools, or modification of user projects, elicitation must explicitly capture implementation-critical decisions.

When applicable, ask for or document:
- Source/destination file mapping.
- Overwrite/default behavior.
- Skip behavior.
- Dry-run behavior.
- Backup behavior.
- Force behavior.
- Confirmation/yes behavior.
- Protected files.
- Active user/project artifacts.
- Path handling.
- Validation environments.
- Greenfield validation case.
- Brownfield validation case.
- MVP vs future behavior.
- Explicit exclusions.

If any applicable decision is not known, record it as an explicit open question with enough precision to be resolved before PRD approval. Do not hide implementation-critical uncertainty under generic assumptions or broad open questions.

## Output template
```markdown
# Elicitation Notes

## Problem Statement
- Problem:
- Evidence:

## Stakeholders
| Stakeholder | Role | Need | Decision Authority |
|---|---|---|---|
| TBD | TBD | TBD | TBD |

## Goals
- Goal:
- Success signal:

## Business Rules
- Rule:
- Source:

## Constraints
- Constraint:
- Impact:

## Assumptions
- Assumption:
- Validation needed:

## Risks
- Risk:
- Impact:
- Mitigation:

## Critical Decisions
| Decision Area | Decision | Source | Status |
|---|---|---|---|
| File mapping | TBD | TBD | Open |
| Overwrite policy | TBD | TBD | Open |
| Validation strategy | TBD | TBD | Open |
| MVP exclusions | TBD | TBD | Open |

## Open Questions
- Question:
- Owner:

## Blocking Questions for PRD
- Question:
- Why it blocks PRD:
- Needed decision:

## Initial Acceptance Signals
- Signal:
- Evidence expected:

## Backlog Update
- Status:
- Current Phase: Discovery
- Last Command: /project:elicit
- Next Command: /project:prd
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/elicitation-notes.md`, `docs/business-context.md`, and `docs/risks-assumptions-constraints.md` exist.
- Problem statement, stakeholders, goals, business rules, constraints, assumptions, risks, open questions, and initial acceptance signals are documented.
- `## Critical Decisions` exists when the product involves file generation, installation, migration, automation, CLI tools, or modification of user projects.
- Implementation-critical unknowns are documented under `## Blocking Questions for PRD` with the needed decision.
- Missing information is represented as assumptions or open questions.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/project:prd`
