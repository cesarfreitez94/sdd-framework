# /project:test

## Purpose
Execute or document the real test run after implementation.

## When to use
Use this command after `/opsx:apply` has implemented the active change.

## Required inputs
- `docs/test-plan.md`.
- `docs/backlog.md`.
- Implemented code.
- Test files.

## Allowed reads
- `docs/test-plan.md`.
- `docs/backlog.md`.
- Implemented code.
- Test files.

## Allowed writes
- `docs/test-results/<change>.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not fake test execution.
- Do not mark tests as passed unless they actually ran and passed.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If tests cannot be run, state why and mark the result as not executed.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/test-results/<change>.md` with real GREEN validation evidence, or a clear not-executed result with the reason.
GREEN validation must reference the prior RED evidence recorded in `docs/test-plan.md` for each related test case.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: none; reads `./docs/test-plan.md` in the target repository
- Runtime output: `./docs/test-results/<change>.md` in the target repository

## Output template
```markdown
# Test Results: <change>

## Test Command Used
- Command:
- Started at:
- Completed at:

## Environment
- OS:
- Runtime:
- Dependencies:

## Result
- Status: PASSED | FAILED | NOT_EXECUTED
- Summary:

## Prior RED Evidence From Test Plan
- Test plan case:
- RED command:
- RED status: EXPECTED_FAIL_RECORDED | NOT_EXECUTED | NOT_APPLICABLE
- RED evidence or justification:

## Passed Tests
- Test:
- Evidence:

## Failed Tests
- Test:
- Failure:
- Evidence:

## Coverage If Available
- Tool:
- Result:
- Evidence:

## Evidence
- Logs:
- Files:
- Screenshots or artifacts:

## Deviations From Test Plan
- Deviation:
- Reason:

## Recommendation
- Recommendation:
- Next command: /project:review if tests pass, otherwise /opsx:apply

## Backlog Update
- Status:
- Current Phase: Delivery
- Last Command: /project:test
- Next Command: /project:review if tests pass, otherwise /opsx:apply
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/test-results/<change>.md` exists.
- Test command, environment, result, prior RED evidence, passed tests, failed tests, coverage if available, evidence, deviations, and recommendation are documented.
- GREEN validation references RED evidence from `docs/test-plan.md`.
- Tests are not faked.
- If tests cannot run, the result is `NOT_EXECUTED` and the reason is documented.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/project:review` if tests pass, otherwise return to `/opsx:apply`.
