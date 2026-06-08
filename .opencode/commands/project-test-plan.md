# /project:test-plan

## Purpose
Create a test plan from the approved OpenSpec proposal/specs and PRD contracts.

## When to use
Use this command after `/opsx:propose` has produced OpenSpec artifacts and before `/opsx:apply`.

## Required inputs
- `docs/prd.md`.
- `docs/backlog.md`.
- `docs/prioritization.md`.
- Approved OpenSpec change artifacts under `openspec/changes/`.

## Allowed reads
- `docs/prd.md`.
- `docs/backlog.md`.
- `docs/prioritization.md`.
- `openspec/changes/`.

## Allowed writes
- `docs/test-plan.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not implement code.
- Do not run tests; this command plans tests, it does not execute them.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/test-plan.md` with CDD coverage and TDD RED/GREEN cases.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Approved OpenSpec Artifacts

OpenSpec artifacts are considered approved for CAFL flow only when all of the following are true:

- OpenSpec proposal/spec/tasks exist.
- The relevant artifact paths or identifiers are recorded.
- The owner or maintainer has confirmed they are acceptable for the next CAFL step.
- No known OpenSpec blocker remains.
- The next CAFL command can reference the artifact evidence.

CAFL commands must not invent OpenSpec approval.
If approval evidence is missing, the command must block or ask for owner/maintainer confirmation.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/test-plan.md`
- Runtime output: `./docs/test-plan.md` in the target repository

## TDD RED Evidence Responsibility

`/project:test-plan` does not execute tests.

Its responsibility is to define the expected RED evidence before `/opsx:apply`.

RED evidence may be captured by the user, test runner, or implementation workflow before `/opsx:apply`.

Allowed RED statuses:

- `EXPECTED_FAIL_RECORDED`: the test was executed before implementation and failed as expected.
- `NOT_EXECUTED`: the RED check could not be executed before implementation; justification is required.
- `NOT_APPLICABLE`: RED does not apply to this case; justification is required.

A test case must not claim TDD RED evidence unless the evidence or justification is recorded.

## Output template
```markdown
# Test Plan

## Scope Under Test
- Scope:
- Out of scope:

## Related Backlog Item
- ID:
- User story:
- Priority:

## Related OpenSpec Change
- Change:
- Proposal:
- Specs:
- Tasks:

## OpenSpec Artifact Evidence

- OpenSpec command:
- Artifact path or identifier:
- Owner/maintainer approval: yes | no
- Approval evidence:
- Known blockers:

## CDD Contracts Covered
| Contract | Source | Covered By |
|---|---|---|
| TBD | docs/prd.md | TBD |

## Test Cases

## Case: <name>
- Given:
- When:
- Then:
- Initial expected state: FAILS (RED)
- Post-implementation expected state: PASSES (GREEN)
- Validates contract:

### RED Evidence

- RED command:
- RED status: EXPECTED_FAIL_RECORDED | NOT_EXECUTED | NOT_APPLICABLE
- RED evidence:
- Justification if not executed:
- Captured by:
- Captured before `/opsx:apply`: yes | no

## Test Data
- Data:
- Source:

## Expected Evidence
- Evidence:
- Location:

## Commands to Run
- Command:
- Notes:

## Assumptions
- Assumption:

## Open Questions
- Question:

## Backlog Update
- Status:
- Current Phase: Specification
- Last Command: /project:test-plan
- Next Command: /opsx:apply
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/test-plan.md` exists.
- Scope under test, related backlog item, related OpenSpec change, CDD contracts covered, test cases, test data, expected evidence, and commands to run are documented.
- Each test case includes the required RED/GREEN TDD block.
- Each automatable test case includes RED evidence status before `/opsx:apply`.
- RED evidence is either actually recorded or explicitly justified as `NOT_EXECUTED` or `NOT_APPLICABLE`.
- Missing information is represented as assumptions or open questions.
- OpenSpec artifacts were read only and not modified.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/opsx:apply`
