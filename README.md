# cafl-framework MVP

cafl-framework is a command-driven software development flow built from Markdown artifacts, OpenCode project commands, and OpenSpec core used as a black box. The MVP keeps the flow small, explicit, and resumable without automation or workflow engines.

## Quickstart
1. Use this repository as a lightweight phase-driven development framework for OpenCode.
2. Make sure OpenCode is installed and configured.
3. Use the project commands in `.opencode/commands/`.
4. Start with `/project:discover-repo`.
5. Use the target repository's `docs/backlog.md` as the MVP state source when resuming work.
6. Use OpenSpec `/opsx:*` commands only during the Specification and Delivery steps shown in the happy path.

## Current Development Status

| Area | Status |
|---|---|
| 9 project flow commands | ✅ Implemented |
| Manual readiness and review gates | ✅ Implemented |
| OpenSpec black-box integration | ✅ Documented |
| Markdown artifact templates | ✅ Moved to `.opencode/templates/docs/` |
| Downstream usage guide | ✅ Documented (manual copy method) |
| Failure and rework flow | ✅ Documented |
| Execution evidence | 🔄 In progress — first downstream validation underway |
| Python installer | ❌ Not implemented — future scope |
| Coordinator agent | ❌ Not implemented — future scope |
| session.yaml | ❌ Not implemented — future scope |
| CI/CD and automated validators | ❌ Not implemented — future scope |

`docs/*.md` in this repository are living framework documentation and evidence,
not project artifacts. Base reusable templates live in `.opencode/templates/docs/`.
`docs/execution-log.md` is mandatory evidence for every downstream validation.

## What the MVP Includes
- 9 cafl project commands under `.opencode/commands/`.
- Markdown documentation templates under `.opencode/templates/docs/`.
- A 13-step happy path from Discovery to Closure.
- Clear separation between cafl commands and OpenSpec commands.
- Backlog-based state tracking for resuming work.
- CDD contract blocks in PRD and test planning.
- TDD RED/GREEN blocks in the test plan.

## What the MVP Excludes
- No Scrum ceremonies.
- No `session.yaml`.
- No CI/CD.
- No automated docs.
- No JSON Schema validators.
- No automated gates.
- No workflow engine.
- No OpenSpec expanded integration.
- No `/project:release-notes` command.
- No agents or agent automation.
- No OpenSpec internals are modified.

Manual review gates and backlog readiness gates exist in the MVP. Automated gates are future scope.

Forbidden or deferred command names may appear in README or command files only as exclusions, future-scope notes, or explicit prohibitions. Their presence in documentation does not mean they are implemented.

## Folder Structure
```text
.opencode/
  commands/
    project-discover-repo.md
    project-elicit.md
    project-prd.md
    project-backlog.md
    project-prioritize.md
    project-test-plan.md
    project-test.md
    project-review.md
    project-retro.md
  templates/
    docs/
      repo-assessment.md
      elicitation-notes.md
      business-context.md
      risks-assumptions-constraints.md
      prd.md
      backlog.md
      prioritization.md
      test-plan.md
      review-report.md
      retro.md
      lessons-learned.md
      execution-log.md

docs/
  execution-log.md
  framework-status.md

README.md
```

OpenSpec may also create and maintain its own `openspec/` folder. cafl commands treat OpenSpec as read-only unless the user explicitly runs OpenSpec commands.

## Source of Truth
| Artifact | Role |
|---|---|
| `README.md` | Current operational guide |
| `.opencode/commands/project-*.md` | Current execution contracts |
| `.opencode/templates/docs/*.md` | Base reusable downstream artifact templates |
| `docs/*.md` | Living framework documentation, status, and downstream validation evidence |
| `docs/execution-log.md` | Real downstream execution, retry, and validation evidence |
| `docs/framework-status.md` | Current cafl-framework repository status |
| `cafl-framework-prd.md` | Historical approved MVP PRD, not current operational source |

> In this repository, `docs/*.md` are living framework docs and evidence, not base project artifact templates. In downstream repositories, copied templates become live generated artifacts.

## Template vs Runtime Artifacts

Base reusable templates live in `.opencode/templates/docs/`.
When setting up a downstream repository, copy `.opencode/templates/docs/*.md`
into the target repository as `docs/*.md` before running project commands.
In this repository, `docs/` contains living framework documentation,
status tracking, and downstream validation evidence — not project artifact templates.

## Applying CAFL to a Downstream Repository

Current MVP usage is manual:

1. Copy `.opencode/commands/project-*.md` into `.opencode/commands/` of the target repo.
2. Copy `.opencode/templates/docs/*.md` into `docs/` of the target repo.
3. Open the target repo in OpenCode.
4. Run `/project:discover-repo` as the first command.
5. Follow `docs/backlog.md` as the MVP state source throughout the project.
6. Use OpenSpec `/opsx:*` only when the happy path reaches Specification or Delivery.

### OpenSpec Preflight

Before running any `/opsx:*` command in the target repository:

- Confirm OpenSpec is installed and configured for that repo.
- Confirm `/opsx:explore`, `/opsx:propose`, `/opsx:apply`, and `/opsx:archive` are available.
- If OpenSpec is unavailable, stop at the current phase and record the blocker
  in `docs/execution-log.md`.
- Do not emulate OpenSpec commands from CAFL.
- Do not modify OpenSpec internals.

A future installer (`cafl-framework-installer`) will automate steps 1 and 2.
Until then, downstream setup is manual.

## Happy Path MVP
```text
/project:discover-repo
/project:elicit
/project:prd
/project:backlog
/project:prioritize
/opsx:explore
/opsx:propose
/project:test-plan
/opsx:apply
/project:test
/project:review
/opsx:archive
/project:retro
```

`/opsx:sync` is optional during Delivery and is not part of the base happy path.

## cafl Commands vs OpenSpec Commands
| Command Type | Responsibility | Ownership |
|---|---|---|
| cafl `/project:*` | Discovery, Definition, test planning, test evidence, review, retro, and Markdown state updates | Implemented by this repository as OpenCode command files |
| OpenSpec `/opsx:*` core | Explore, propose, apply, optionally sync, and archive technical changes | External black box; documented only |
| Stack commands | Project-specific build, test, lint, or runtime commands | Provided by the target repository |

cafl commands must not duplicate OpenSpec functionality and must not modify OpenSpec internals.

## OpenSpec Core Commands Used
These commands are documented as external black-box capabilities. They are not implemented by cafl-framework.

| Command | MVP Role | Happy Path |
|---|---|---|
| `/opsx:explore` | Investigate before committing to a technical change | Yes |
| `/opsx:propose` | Produce proposal, specs, design, and tasks | Yes |
| `/opsx:apply` | Implement from OpenSpec tasks | Yes |
| `/opsx:sync` | Sync delta specs without archiving | Optional during Delivery only |
| `/opsx:archive` | Close and archive the change | Yes |

## OpenSpec Expanded Commands Deferred to v1.1
These commands are future scope only and are not integrated into the MVP.

| Command | Deferred Purpose |
|---|---|
| `/opsx:new` | Formal change initialization |
| `/opsx:continue` | Incremental artifact continuation |
| `/opsx:ff` | Fast-forward artifact generation |
| `/opsx:verify` | OpenSpec verification flow |
| `/opsx:bulk-archive` | Bulk archive completed changes |
| `/opsx:onboard` | Brownfield OpenSpec onboarding |

## Backlog as MVP State Source
In downstream repositories, `docs/backlog.md` is the only MVP state source. Do not create `session.yaml` in the MVP.

Every cafl command must update `docs/backlog.md` when the file exists. This update is limited to the active item columns: `Status`, `Current Phase`, `Last Command`, and `Next Command`. No command may rewrite unrelated backlog content. If `docs/backlog.md` does not exist yet, the command must state that backlog update was skipped.

Allowed status values are:
- `Pending`
- `In Progress`
- `Blocked`
- `Completed`

## Failure, Rework, and Retry Flow

| Failure point | Required action | Retry target | Evidence artifact |
|---|---|---|---|
| `/project:elicit` → NOT_READY | Gather missing context, re-elicit | `/project:elicit` | `docs/elicitation-notes.md` |
| `/project:prd` → NOT_READY | Resolve blocking questions | `/project:elicit` | `docs/prd.md` |
| `/project:backlog` → PRD NOT_READY | Stop backlog generation | `/project:prd` | `docs/backlog.md` |
| `/project:test-plan` → OpenSpec artifacts missing | Stop, repair OpenSpec proposal | `/opsx:propose` | `docs/test-plan.md` |
| `/project:test` → FAILED or NOT_EXECUTED | Rework implementation | `/opsx:apply` | `docs/test-results/` |
| `/project:review` → CHANGES_REQUIRED or BLOCKED | Rework before archive | `/opsx:apply` | `docs/review-report.md` |
| `/project:review` → repeated failure | Review test-plan and PRD alignment | `/project:test-plan` or `/project:prd` | `docs/review-report.md` |
| Downstream validation exposes framework weakness | Record in execution-log, improve command | `docs/execution-log.md` → `.opencode/commands/project-*.md` | `docs/execution-log.md` |

## Downstream Validation Loop
1. Apply the happy path in a real downstream repo.
2. Record every command execution in `docs/execution-log.md`.
3. If a command fails or produces weak output, identify the root cause.
4. Apply the fix in `.opencode/commands/project-*.md`.
5. Re-run the same downstream step to validate.
6. If validated, commit the framework change with reference to the downstream
   repo and the execution-log entry.

## v1.1 Persistence Options
MVP uses Option C in simple form: `docs/backlog.md` with state columns. v1.1 should evaluate Option B for larger or multi-session projects.

### Option A — Markdown status section
Use a dedicated Markdown status block, for example in `docs/sprint-status.md`, to record current phase, active item, last command, next command, and blocked state.

Pros:
- Simple and readable.
- No parser required.
- Easy for any agent or human to inspect.

Cons:
- Manual updates can become inconsistent.
- Status may drift from backlog if both are edited separately.
- Less suitable for larger projects.

### Option B — YAML session state
Use `.cafl/session.yaml` in v1.1 to store structured state such as active item, phase, last command, next command, status, blocked state, and artifact status.

Pros:
- Structured and parseable.
- Better for automation.
- Easier to validate in later versions.
- Better fit for larger projects.

Cons:
- Requires `/project:session-save` and `/project:session-restore`.
- Adds one more state file.
- Requires stricter update discipline.

### Option C — Extended backlog state
Keep `docs/backlog.md` as the single state source and add more detailed state columns as needed.

Pros:
- Single source of truth.
- No additional state file.
- Works well for MVP and small projects.

Cons:
- Backlog can become dense.
- Harder to manage for large projects.
- Table edits may become fragile.

## CDD Placement
CDD starts in `docs/prd.md` under `## Expected Contracts`.

Each contract uses this block:

```markdown
### Contract: <name>
- Source:
- Input:
- Output:
- Rules:
- Expected errors:
- Edge cases:
- Validation method:
- Related acceptance criteria:
```

OpenSpec specs may reference or refine these contracts, but cafl does not modify OpenSpec internals. `docs/test-plan.md` maps test cases back to contracts, and `docs/review-report.md` checks contract compliance.

## TDD Placement
TDD starts in `docs/test-plan.md` after `/opsx:propose` and before `/opsx:apply`.

Each test case uses this block:

```markdown
## Case: <name>
- Given:
- When:
- Then:
- Initial expected state: FAILS (RED)
- Post-implementation expected state: PASSES (GREEN)
- Validates contract:
```

`/project:test` records the real execution result. Tests must not be faked.

## How to Resume Work
1. Open `docs/backlog.md`.
2. Find the active item by `Status` and `Current Phase`.
3. Read `Last Command` to understand what was completed.
4. Run the command shown in `Next Command`.
5. Read the phase artifact linked to that command before writing anything.

## Definition of MVP Done
- All 9 command files exist under `.opencode/commands/`.
- All required template files under `.opencode/templates/docs/` and living framework docs under `docs/` exist.
- `README.md` documents the happy path.
- OpenSpec internals are not modified.
- No v1.1 or v1.2 functionality is implemented.
- Backlog template includes state columns.
- PRD template includes the CDD contract block.
- Test plan template includes the TDD RED/GREEN block.
- Review template includes CDD/TDD evidence checks.
- Retro template includes lessons learned.
- `git status` shows only expected files changed or created.

## Language Policy
Repository files generated by cafl-framework MVP must be written in English.

Final assistant/user-facing reports may be written in Spanish.

Approved source PRD files that existed before implementation may remain in their original language and are not considered generated MVP files.

## Master Flow Map
| Phase | Original Item | OpenSpec Coverage | MVP Command | Output |
|---|---|---|---|---|
| Discovery | Explore repo | Not covered | `/project:discover-repo` | `docs/repo-assessment.md` |
| Discovery | BABOK elicitation | Not covered | `/project:elicit` | `docs/elicitation-notes.md` |
| Definition | PRD | Not covered | `/project:prd` | `docs/prd.md` |
| Definition | Backlog | Not covered | `/project:backlog` | `docs/backlog.md` |
| Definition | Prioritization | Not covered | `/project:prioritize` | `docs/prioritization.md` |
| Specification | OpenSpec / SDD | Primary | `/opsx:explore` + `/opsx:propose` | proposal, specs, design, tasks |
| Specification | Acceptance criteria | Partial | Acceptance and contract blocks in specs | `openspec/changes/<change>/specs/` |
| Specification | Test plan | Not covered | `/project:test-plan` | `docs/test-plan.md` |
| Delivery | Apply | Covered | `/opsx:apply` | Implemented code |
| Delivery | Test | Not covered | `/project:test` | `docs/test-results/<change>.md` |
| Delivery | Documentation | Out of MVP | - | v1.2 |
| Delivery | CI/CD | Out of MVP | - | v1.2 |
| Closure | Review | Not covered | `/project:review` | `docs/review-report.md` |
| Closure | Archive | OpenSpec core | `/opsx:archive` | Archived OpenSpec change |
| Closure | Release notes | Out of MVP | Temporarily summarized in `/project:review` or `/project:retro` if applicable | v1.1 |
| Closure | Retro / lessons learned | Not covered | `/project:retro` | `docs/retro.md`, `docs/lessons-learned.md` |

## Usage Notes
- Keep outputs concise but complete.
- Use Markdown outputs only.
- Do not commit or push from cafl commands.
- Do not invent missing information; write assumptions and open questions.
- Preserve separation between Discovery, Definition, Specification, Delivery, and Closure.
- Use `/opsx:sync` only when explicitly needed during Delivery; it is not part of the base happy path.
