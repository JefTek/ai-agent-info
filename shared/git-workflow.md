# Git Workflow Instructions

Follow this Git workflow for all repository changes.

## Core rules

- Never commit directly to `main`, `master`, or another protected branch.
- Never force-push a protected branch.
- Never merge a pull request unless explicitly authorized.
- Never modify, discard, reset, stash, or overwrite changes that you did not create.
- Never use another agent's branch or working directory.
- Use one short-lived branch for one task.
- Keep changes narrowly scoped to the requested task.
- Do not include unrelated refactoring, formatting, dependency updates, or cleanup.
- Do not commit secrets, credentials, tokens, generated artifacts, environment files, or local configuration.
- Do not bypass tests, hooks, branch protection, or CI checks.
- Do not use destructive Git commands unless explicitly authorized.

Destructive commands include, but are not limited to:

- `git reset --hard`
- `git clean -fd`
- `git checkout -- .`
- `git restore .`
- `git push --force`
- deleting branches you did not create
- rewriting commits belonging to another contributor

## Starting the task

1. Inspect the repository state:

   ```bash
   git status --short
   git branch --show-current
   git remote -v
   ```

2. Read the repository instructions, including any applicable:

   - `AGENTS.md`
   - `README.md`
   - `CONTRIBUTING.md`
   - development documentation
   - test configuration
   - lint and formatting configuration
   - CI configuration

3. Fetch the latest remote state:

   ```bash
   git fetch --prune origin
   ```

4. Create a new branch from the current remote default branch:

   ```bash
   git switch -c agent/<short-task-name> origin/<default-branch>
   ```

5. Confirm the working tree is clean before making changes:

   ```bash
   git status --short
   ```

## During the task

- Check status before editing, committing, rebasing, merging, or switching branches.
- Only edit files needed for the requested task.
- Preserve user and other agent changes.
- If unrelated changes are present, leave them untouched and ask for guidance when they block the task.
- Prefer small, reviewable commits that describe the completed work.
- Run the repository's existing validation commands for the files or systems you changed.

## Committing changes

1. Review the final diff:

   ```bash
   git status --short
   git diff
   ```

2. Ensure no secrets, generated artifacts, local configuration, or unrelated changes are staged.
3. Stage only files you intentionally changed:

   ```bash
   git add <paths>
   ```

4. Commit with a concise message that describes the task:

   ```bash
   git commit -m "<short task summary>"
   ```

## Pull requests

- Push only the task branch.
- Open one pull request for the task branch.
- Summarize the purpose, notable changes, and validation performed.
- Do not merge the pull request unless explicitly authorized.
- If CI fails, inspect the logs, fix only task-related issues, and rerun validation.

## If something goes wrong

- Stop before using destructive commands.
- Do not discard or overwrite changes you did not create.
- Explain the problem, current branch, working tree state, and safest next options.
- Ask for explicit authorization before any destructive recovery step.
   git switch -c agent/<task-id>-<short-description> origin/main
   ```

   Use `origin/master` or the repository's configured default branch when it is not `main`.

5. When multiple agents may work concurrently, use a dedicated Git worktree:

   ```bash
   git worktree add <isolated-path> \
     -b agent/<task-id>-<short-description> origin/main
   ```

   Do not continue if the intended worktree or branch is already being used by another agent.

## Before implementation

- Understand the requested behavior and acceptance criteria.
- Inspect the relevant implementation and tests.
- Search for established repository patterns before creating new abstractions.
- Identify the smallest set of files that should change.
- Record the starting test status when relevant.
- Do not change code until you understand how the affected area currently works.

If the working tree contains pre-existing changes, preserve them. Do not modify or include them in your commits unless they are explicitly part of the assigned task.

## During implementation

- Make the smallest complete change that satisfies the task.
- Follow existing architecture, naming, formatting, and testing conventions.
- Add or update tests for changed behavior.
- Avoid broad rewrites.
- Avoid unrelated dependency upgrades.
- Do not edit lockfiles unless dependency changes are required.
- Do not suppress warnings or disable checks merely to make validation pass.
- Stop and report any unexpected repository state, ambiguous destructive operation, security concern, or conflict with existing work.

## Commits

Create logical, reviewable commits.

Before each commit:

```bash
git status --short
git diff
git diff --check
```

Stage files deliberately:

```bash
git add <specific-file-or-directory>
```

Do not use `git add .` without first confirming that every changed file belongs to the task.

Use descriptive commit messages, preferably Conventional Commit format:

```text
feat(scope): add requested behavior
fix(scope): correct specific defect
test(scope): cover edge case
docs(scope): document configuration
refactor(scope): simplify implementation without behavior change
```

Each commit must:

- represent one coherent change
- leave the repository in a valid state
- contain no secrets or unrelated files
- explain intent rather than merely listing edited files

## Validation

Run the repository's documented validation commands. This normally includes:

- formatting checks
- linting
- static analysis or type checking
- unit tests
- integration tests relevant to the change
- build or packaging checks

Do not claim a check passed unless you actually ran it and observed a successful result.

When a check cannot run, report:

- the exact command
- the reason it could not run
- the relevant error
- whether the failure appears related to the change

## Final review

Before pushing, inspect all branch changes:

```bash
git status --short
git diff --check
git diff origin/main...HEAD
git log --oneline origin/main..HEAD
```

Confirm that:

- only intended files changed
- acceptance criteria are satisfied
- tests cover the new behavior
- no debug code remains
- no temporary files are included
- no secrets or credentials are present
- documentation is updated when needed
- commits are understandable and appropriately scoped

## Synchronizing with the base branch

Immediately before pushing or updating the pull request:

```bash
git fetch origin
git rebase origin/main
```

Only rebase when this branch is owned exclusively by this task. Do not rewrite a shared branch.

If conflicts occur:

- resolve only conflicts you understand
- rerun validation after resolving them
- do not guess at another contributor's intent
- stop and report the conflict when safe resolution is unclear

Never use force push unless explicitly authorized. When authorization is provided for an agent-owned branch, use:

```bash
git push --force-with-lease
```

Never use plain `--force`.

## Push and pull request

Push the task branch:

```bash
git push -u origin agent/<task-id>-<short-description>
```

Create or update a pull request, but do not merge it unless explicitly authorized.

The pull request description must include:

### Summary

A concise explanation of what changed and why.

### Changes

The important implementation details.

### Validation

Every command run and its result.

### Risks

Compatibility concerns, migrations, security implications, limitations, or known edge cases.

### Files

Any generated files, database migrations, configuration changes, or dependency changes.

### Issue

The related issue or task identifier.

## Completion report

At completion, report:

- branch name
- worktree path, when applicable
- commit hashes and messages
- files changed
- concise implementation summary
- tests and checks run
- results of each check
- pull-request link, when created
- known risks or unresolved concerns
- any actions requiring human review

Do not describe the task as complete while tests are failing, conflicts remain unresolved, or required acceptance criteria have not been verified.
