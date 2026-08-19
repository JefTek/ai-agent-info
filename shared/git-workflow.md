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
