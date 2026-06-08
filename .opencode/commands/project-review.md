# /project:review

## Purpose
Perform final technical and functional review before closing the change.

## When to use
Use this command after `/project:test` has passing evidence and before `/opsx:archive`.

## Required inputs
- `docs/prd.md`.
- `docs/backlog.md`.
- `docs/test-plan.md`.
- `docs/test-results/`.
- OpenSpec artifacts.
- Git diff or status if available.

## Allowed reads
- `docs/prd.md`.
- `docs/backlog.md`.
- `docs/test-plan.md`.
- `docs/test-results/`.
- OpenSpec artifacts.
- Git diff/status if available.

## Allowed writes
- `docs/review-report.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not implement fixes during review; document required changes instead.
- Do not create release-notes command output.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/review-report.md` with technical, functional, CDD, and TDD evidence review.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/review-report.md`
- Runtime output: `./docs/review-report.md` in the target repository

## Blocking Conditions

Stop review and return BLOCKED if any of the following are missing or incomplete:

- `docs/test-results/` does not exist or contains no results.
- OpenSpec change artifact is missing (no `/opsx:apply` was run or archived).
- CDD mapping between PRD contracts and test cases is absent.
- TDD evidence (RED → GREEN progression) is not documented.
- `docs/backlog.md` item under review is not aligned with `docs/prd.md`.

If BLOCKED, output:

REVIEW BLOCKED Missing: <list what is missing> Required action: <what must be completed before re-running review> Retry: /project:review after completing required action

Do not approve, partially approve, or suggest workarounds for blocked items.

## Output template
```markdown
# Review Report

## Scope Reviewed
- Scope:
- Out of scope:

## Backlog Item Reviewed
- ID:
- Status:

## OpenSpec Change Reviewed
- Change:
- Ready to archive: yes | no | unknown

## PRD Alignment
- Finding:
- Evidence:

## CDD Contract Compliance
- Contract:
- Compliant: yes | no | partial | unknown
- Evidence:

## TDD Evidence Review
- Test plan case:
- Result:
- Evidence:

## Code Quality Notes
- Note:
- Severity:

## Architecture Notes
- Note:
- Risk:

## Security Notes
- Note:
- Risk:

## Documentation Notes
- Note:
- Gap:

## Release Notes Summary If Applicable
- Summary:
- Note: Formal `/project:release-notes` is out of MVP scope.

## Verdict
- Verdict: APPROVED | APPROVED_WITH_NOTES | CHANGES_REQUIRED | BLOCKED
- Reason:
- Next command: /opsx:archive if approved, otherwise /opsx:apply

## Backlog Update
- Status:
- Current Phase: Closure
- Last Command: /project:review
- Next Command: /opsx:archive if approved, otherwise /opsx:apply
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/review-report.md` exists.
- Scope, backlog item, OpenSpec change, PRD alignment, CDD compliance, TDD evidence, code quality, architecture, security, documentation, release notes summary if applicable, and verdict are documented.
- Verdict is one of `APPROVED`, `APPROVED_WITH_NOTES`, `CHANGES_REQUIRED`, or `BLOCKED`.
- OpenSpec artifacts were read only and not modified.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
- If `APPROVED` or `APPROVED_WITH_NOTES`: `/opsx:archive`.
- If `CHANGES_REQUIRED` or `BLOCKED`: return to `/opsx:apply`.
