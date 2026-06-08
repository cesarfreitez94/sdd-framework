> CAFL_BACKLOG_TEMPLATE_UNINITIALIZED
> Base template — part of cafl-framework.
> Copy to `docs/` in the target repository before running project commands.
> `/project:backlog` must replace the placeholder row below with real backlog items from `docs/prd.md`.

# Product Backlog

The backlog is the MVP state source. Resume work by reading the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`.

## Backlog Table

| ID | User Story | Acceptance Criteria | Priority | Status | Current Phase | Last Command | Next Command |
|---|---|---|---|---|---|---|---|
| PLACEHOLDER-BACKLOG-ID | TEMPLATE placeholder user story example. Replace with real PRD-derived backlog content. | TBD | TBD | Pending | Definition | /project:backlog | /project:prioritize |

## Status Values
- Pending
- In Progress
- Blocked
- Completed

## Update Rule
Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content.

## Notes
- Assumptions: TBD
- Open questions: TBD
