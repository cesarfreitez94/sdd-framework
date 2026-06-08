# /project:discover-repo

## Purpose
Explore the current repository before defining or implementing changes.

## When to use
Use this command at the start of the cafl flow, before elicitation, PRD writing, backlog creation, OpenSpec work, or implementation.

## Required inputs
- Current repository working tree.
- Any existing README, docs, configuration, project files, tests, CI files, and OpenSpec folder if present.

## Allowed reads
- Repository tree.
- README files.
- Existing `docs/` files.
- Configuration files.
- Package or project files.
- Test folders.
- CI files if present.
- OpenSpec folder if present, read-only.

## Allowed writes
- `docs/repo-assessment.md`.
- `docs/backlog.md`, only to update `Status`, `Current Phase`, `Last Command`, and `Next Command` for the active item when the file exists.

## Forbidden actions
- Do not commit.
- Do not push.
- Do not modify OpenSpec internals.
- Do not implement code changes.
- Do not create automation, agents, workflow engines, CI/CD, JSON Schema validators, Scrum ceremonies, or `session.yaml`.
- Do not invent missing information. If information is missing, write assumptions and open questions.
- Do not update files outside the allowed writes.

## Expected output
Create or update `docs/repo-assessment.md` with a concise but complete repository assessment.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

## Template Source and Runtime Output

- Template source: `.opencode/templates/docs/repo-assessment.md`
- Runtime output: `./docs/repo-assessment.md` in the target repository

## Output template
```markdown
# Repository Assessment

## Repository Overview
- Summary:
- Main directories:
- Notable files:

## Detected Stack
- Languages:
- Frameworks:
- Package managers:
- Runtime or platform:

## Architecture Notes
- Structure:
- Boundaries:
- Integration points:

## Existing Tests
- Test folders:
- Test tools:
- Current coverage evidence:

## Existing Docs
- README:
- Docs:
- Gaps:

## Existing CI/CD
- CI files found:
- Release or deployment notes:
- Gaps:

## Risks
- Risk:
- Impact:
- Mitigation:

## Technical Debt
- Item:
- Evidence:
- Impact:

## Constraints
- Constraint:
- Source:

## Assumptions
- Assumption:

## Open Questions
- Question:

## Backlog Update
- Status:
- Current Phase:
- Last Command: /project:discover-repo
- Next Command: /project:elicit
- Note: If `docs/backlog.md` did not exist, state that the backlog update was skipped.

## Recommended Next Step
Run `/project:elicit`.
```

## Acceptance criteria
- `docs/repo-assessment.md` exists.
- The repository overview, stack, architecture, tests, docs, CI/CD, risks, technical debt, constraints, and recommended next step are documented.
- Missing information is represented as assumptions or open questions.
- OpenSpec content, if present, was read only and not modified.
- `docs/backlog.md` was updated only in the allowed active item columns when it exists, or the skipped update was stated.

## Next command
`/project:elicit`
