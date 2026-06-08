# /project:backlog

## Purpose
Transform the PRD into an actionable product backlog.

## When to use
Use this command after `docs/prd.md` exists and before prioritization.

## Required inputs
- `docs/prd.md`.

## Allowed reads
- `docs/prd.md`.

## Allowed writes
- `docs/backlog.md`, to create the initial backlog table from `docs/prd.md` when the file does not exist.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not create prioritization, test, review, or retro files.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/backlog.md` as the MVP state source with actionable user stories and required state columns.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/backlog.md`
- Runtime output: `./docs/backlog.md` in the target repository

## Preflight Check

Before generating the backlog:

1. Read `docs/prd.md`.
2. Verify `## Backlog Readiness`.
3. If `Status: READY`, proceed.
4. If `Status: NOT_READY`, stop and output:
   `BLOCKED: PRD is NOT_READY. Resolve all Blocking Open Questions before generating backlog.`
5. If `## Backlog Readiness` is missing, stop and route back to `/project:prd`.
6. Do not encode ambiguous PRD items into backlog stories.

## Output template
```markdown
# Product Backlog

The backlog is the MVP state source. Update only the active item state columns when progressing through commands.

## Backlog Table

| ID | User Story | Acceptance Criteria | Priority | Status | Current Phase | Last Command | Next Command |
|---|---|---|---|---|---|---|---|
| US-001 | As a [role], I want [action] so that [benefit]. | AC-001; AC-002 | TBD | Pending | Definition | /project:backlog | /project:prioritize |

## Status Values
- Pending
- In Progress
- Blocked
- Completed

## Notes
- Assumptions:
- Open questions:

## Backlog Update
- Status: In Progress
- Current Phase: Definition
- Last Command: /project:backlog
- Next Command: /project:prioritize
```

## Acceptance criteria
- `docs/backlog.md` exists.
- Backlog generation only proceeds when `docs/prd.md` declares `## Backlog Readiness` with `Status: READY`.
- If `docs/prd.md` declares `Status: NOT_READY`, the command stops and outputs the required blocked message.
- If `## Backlog Readiness` is missing, the command stops and routes back to `/project:prd`.
- The backlog table includes exactly these columns: `ID`, `User Story`, `Acceptance Criteria`, `Priority`, `Status`, `Current Phase`, `Last Command`, and `Next Command`.
- Status values are limited to `Pending`, `In Progress`, `Blocked`, and `Completed`.
- The backlog is documented as the MVP state source.
- Missing information is represented as assumptions or open questions.
- Ambiguous PRD items are not encoded into backlog stories.
- Existing unrelated backlog content is not rewritten when only state needs to change.

## Next command
`/project:prioritize`
