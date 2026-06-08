# Framework Status

## Current State

- Status: MVP implemented; downstream validation in progress.
- Current macro focus: stabilizing operational contracts and validating the framework in real downstream repositories.
- Last reviewed: 2026-06-08.
- Next recommended action: re-run repository audit, then continue downstream validation if no critical fixes remain.

## Implemented

- 9 project delivery commands under `.opencode/commands/`
- Manual readiness and review gates
- OpenSpec black-box integration model
- Template/runtime artifact separation
- Downstream usage guide
- Failure and rework flow documentation
- Execution log with initial downstream evidence

## In Progress

- cafl-framework-installer downstream validation
- Operational contract hardening

## Pending

- Python installer
- Framework evolution commands (`/framework:*`)
- Coordinator agent
- Elicitation review command
- Question-enabled elicitation
- session persistence evaluation

## Blocked

- None currently

## Notes

- `docs/framework-status.md` tracks the framework repository status only.
- `docs/backlog.md` must not be used as the framework repository status source.
- Maintainer-only actions such as commit, push, and declaring validation accepted must be performed by the owner/maintainer, not by project commands.
