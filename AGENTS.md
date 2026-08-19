# Agent instructions

Instructions for agents working in this repository. This repository is a
library of agent guidance, so it applies its own guidance to itself: the
documents in `shared/` are normative here, not just published for others.

## What this repository is

A workspace-neutral library of notes, tips, tricks, skills, and agent patterns
for Claude and GitHub Copilot. It contains documentation only — there is no
source code, build, dependency manifest, or test suite.

## Before changing anything

1. Read [CONTRIBUTING.md](CONTRIBUTING.md) for where content belongs and the
   format each kind of entry uses.
2. Read the README of the area being changed: [claude-ai](claude-ai/README.md),
   [copilot](copilot/README.md), or [shared](shared/README.md).
3. Follow [shared/git-workflow.md](shared/git-workflow.md) for branching,
   commits, review, and pull requests. Its rules apply to work in this
   repository.

## House rules

- Keep entries short, actionable, and safe to hand to an agent verbatim.
- Use bracketed placeholders such as `[feature]` and `[path]` so entries stay
  reusable across workspaces.
- Wrap prose at roughly 78 characters, matching the existing files. Tables,
  URLs, and code blocks may exceed it.
- Use sentence case for headings, except where a proper noun or a term from an
  external specification requires otherwise.
- Do not claim a document was verified against an external source without
  actually checking it, and state the version any external specification was
  read at.

## Recording changes

Add notable changes to [docs/CHANGELOG.md](docs/CHANGELOG.md) under
**Unreleased**, in the same pull request that makes them. Notable means a
reader of the repository would want to know: new or removed documents,
reorganized content, changed conventions. Typo fixes and rewrapping do not
qualify.

Problems found but not fixed belong in [docs/ISSUES.md](docs/ISSUES.md) if a
reader should know about them, or in GitHub Issues if they need discussion.
Do not leave a known defect recorded only in a pull request description.

## Keeping the indexes in sync

Adding or renaming a standalone document under `shared/` means updating its
**Contents** entry in [shared/README.md](shared/README.md#contents). The root
[README.md](README.md) names the standalone documents too; keep it current or
the two will drift.

## Validation

There is no build or test suite. Markdown is linted, and the same checks run
in CI on every pull request:

```bash
npx markdownlint-cli2
```

Rules live in `.markdownlint-cli2.jsonc`, so a local run and CI check exactly
the same thing. CI additionally runs a link check over relative paths and
in-page anchors; external URLs are not requested.

The linter does not catch everything. Before opening a pull request, also:

- Re-read the rendered diff for spliced or duplicated content, which past
  merges have introduced twice.
- Confirm no secrets, credentials, or personal data are present.
- Confirm any claim that a document was checked against an external source
  was actually checked.

Do not report a check as passing unless it was actually run.
