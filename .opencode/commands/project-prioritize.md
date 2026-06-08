# /project:prioritize

## Purpose
Prioritize the backlog using a simple MoSCoW approach.

## When to use
Use this command after the backlog exists and before OpenSpec exploration.

## Required inputs
- `docs/backlog.md`.
- `docs/prd.md`.

## Allowed reads
- `docs/backlog.md`.
- `docs/prd.md`.

## Allowed writes
- `docs/prioritization.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not run or invoke OpenSpec commands.
- Do not create test, review, or retro files.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create `docs/prioritization.md` and update backlog state columns for the active item.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/prioritization.md`
- Runtime output: `./docs/prioritization.md` in the target repository

## Output template
```markdown
# Prioritization

## Prioritization Method
- Method: MoSCoW
- Inputs used:
- Decision owner:

## Must Have
- Backlog ID:
- Rationale:

## Should Have
- Backlog ID:
- Rationale:

## Could Have
- Backlog ID:
- Rationale:

## Won't Have
- Backlog ID:
- Rationale:

## Rationale
- Decision:
- Evidence:
- Trade-off:

## Updated Backlog Priorities
| ID | Priority | Reason |
|---|---|---|
| TBD | TBD | TBD |

## Backlog Update
- Status:
- Current Phase: Definition
- Last Command: /project:prioritize
- Next Command: /opsx:explore
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.
```

## Acceptance criteria
- `docs/prioritization.md` exists.
- Must have, Should have, Could have, and Won't have groups are documented.
- The rationale for prioritization is documented.
- Backlog priorities are updated or explicitly documented as pending validation.
- Missing information is represented as assumptions or open questions.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/opsx:explore`
