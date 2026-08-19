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

## Keeping the indexes in sync

Adding or renaming a standalone document under `shared/` means updating its
**Contents** entry in [shared/README.md](shared/README.md#contents). The root
[README.md](README.md) names the standalone documents too; keep it current or
the two will drift.

## Validation

There is no build, lint, or test suite. Before opening a pull request:

- Re-read the rendered diff for spliced or duplicated headings, which past
  merges have introduced.
- Confirm every relative link and in-page anchor resolves.
- Confirm no secrets, credentials, or personal data are present.

Do not report a check as passing unless it was actually run.
