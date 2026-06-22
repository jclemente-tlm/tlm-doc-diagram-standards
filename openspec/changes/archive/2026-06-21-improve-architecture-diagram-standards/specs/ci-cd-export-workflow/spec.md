# Delta Spec: ci-cd-export-workflow

## ADDED Requirements

### Requirement: Trigger on pull_request with drawio paths

The shared GitHub Actions workflow MUST trigger on `pull_request` events that change at least one file matching `**/*.drawio`. The workflow MUST NOT trigger on direct pushes to `main`, on `pull_request_target`, or on events that do not modify a `.drawio` file.

#### Scenario: PR modifying a drawio file triggers the workflow

- GIVEN a pull request modifies `docs/architecture/payment.drawio`
- WHEN the PR is opened, synchronized, or reopened
- THEN the workflow run starts

#### Scenario: PR without drawio changes does not trigger

- GIVEN a pull request modifies only `README.md` and no `.drawio` file
- WHEN the PR is opened
- THEN the workflow does not start

#### Scenario: Direct push to main does not trigger

- GIVEN a commit pushed directly to `main` modifies `docs/architecture/payment.drawio`
- WHEN the push completes
- THEN the workflow does not start on `main`

### Requirement: Workflow commits PNGs to the same PR branch

When the export step produces one or more PNGs, the workflow MUST commit them to the same branch that originated the pull request. The workflow MUST NOT open a new pull request, MUST NOT push to a different branch, and MUST NOT create an issue. The PR diff MUST show both the original `.drawio` change and the auto-generated `.png` files.

#### Scenario: Generated PNGs land in the originating PR

- GIVEN the export step generates `payment.png` for the PR that modified `payment.drawio`
- WHEN the export completes successfully
- THEN a commit is pushed to the PR's source branch containing `payment.png`
- AND the PR diff shows both `payment.drawio` and `payment.png`

#### Scenario: Workflow does not open a secondary PR

- GIVEN the export workflow has pushed PNGs to the PR branch
- WHEN the push completes
- THEN no new PR is opened
- AND no issue is created

### Requirement: No commits when no PNG changes

The workflow MUST NOT create empty commits or commits whose only changes are byte-identical to PNGs already present in the PR branch. When rendered output matches the committed PNG, the workflow SHALL skip the commit and exit successfully.

#### Scenario: Byte-identical PNG produces no commit

- GIVEN the export produces a PNG byte-identical to the one already committed in the PR branch
- WHEN the export completes
- THEN no commit is created
- AND the workflow exits with success

#### Scenario: Mixed unchanged and changed files

- GIVEN the PR modifies `a.drawio` (PNG unchanged) and `b.drawio` (PNG changed)
- WHEN the export completes
- THEN exactly one commit is created
- AND the commit contains only the new `b.png`

### Requirement: PR checklist item for manual validation

The pull request template defined in the shared workflow repository MUST include a manual checklist item asking reviewers to verify the auto-generated PNG reflects the diagram changes. The item SHALL appear unchecked for every PR that modifies a `.drawio` file.

#### Scenario: Reviewer sees the PNG validation item

- GIVEN a pull request modifies `docs/architecture/payment.drawio`
- WHEN a reviewer opens the PR
- THEN the PR description includes a checklist item about PNG update validation
- AND the item is unchecked

### Requirement: Main branch stays clean

The workflow MUST NOT push commits, tags, or references directly to `main` under any circumstance. All auto-generated PNGs MUST be delivered exclusively through a pull request branch.

#### Scenario: Direct push to main is ignored

- GIVEN a commit pushed directly to `main` modifies `payment.drawio`
- WHEN the push completes
- THEN no bot commit appears on `main`

#### Scenario: Bot commits never land on main

- GIVEN any execution of the export workflow
- WHEN the workflow produces a commit
- THEN the commit's branch is the PR's source branch
