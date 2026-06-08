# /project:retro

## Purpose
Capture lessons learned and process improvements after a completed flow.

## When to use
Use this command after an approved or approved-with-notes review has been archived with `/opsx:archive`.

## Required inputs
- `docs/review-report.md`.
- `docs/test-results/`.
- `docs/backlog.md`.
- Archived OpenSpec artifacts if available.

## Allowed reads
- `docs/review-report.md`.
- `docs/test-results/`.
- `docs/backlog.md`.
- Archived OpenSpec artifacts if available, read-only.

## Allowed writes
- `docs/retro.md`.
- `docs/lessons-learned.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not introduce Scrum ceremonies into the MVP.
- Do not create `/project:release-notes` output.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/retro.md` and `docs/lessons-learned.md` with concise lessons and follow-up actions.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/retro.md`, `.opencode/templates/docs/lessons-learned.md`
- Runtime output: `./docs/retro.md`, `./docs/lessons-learned.md` in the target repository

## Output template
```markdown
# Retro

## What Worked
- Item:
- Evidence:

## What Failed
- Item:
- Impact:

## What Was Confusing
- Item:
- Cause:

## Process Improvements
- Improvement:
- Owner:

## Command Improvements
- Command:
- Improvement:

## OpenSpec Integration Issues
- Issue:
- Impact:
- Suggested follow-up:

## Follow-up Actions
- Action:
- Owner:
- Due:

## Items to Consider for v1.1
- Item:
- Reason:

## Backlog Update
- Status: Completed
- Current Phase: Closure
- Last Command: /project:retro
- Next Command: None
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/retro.md` exists.
- `docs/lessons-learned.md` exists.
- What worked, what failed, what was confusing, process improvements, command improvements, OpenSpec integration issues, follow-up actions, and v1.1 considerations are documented.
- The retro captures lessons learned without introducing Scrum ceremonies.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
None. Flow complete.
