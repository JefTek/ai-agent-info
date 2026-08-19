# Known issues

Open problems and accepted limitations, tracked here so they stay visible
between sessions.

This file is for issues worth carrying in the repository itself: accepted
limitations, decisions awaiting a call, and drift risks that a reader of the
code should know about. Anything needing discussion, assignment, or a due date
belongs in [GitHub Issues](https://github.com/JefTek/ai-agent-info/issues)
instead. When an entry exists in both places, the GitHub issue is
authoritative and this entry should link to it.

Status values: **open**, **accepted** (a limitation the project is choosing to
live with), or **resolved** (kept briefly for context, then removed).

## ISS-1

**Canonical content of `shared/git-workflow.md` was chosen, not reviewed** —
open.

The merge of [#2] left the document as two interleaved versions. The repair in
[#6] kept the longer version as canonical and dropped three sections
(`During the task`, `Committing changes`, `Pull requests`) as duplicative,
after folding their two unique bullets into the sections that supersede them.

The alternative readings — keep the shorter version, or keep both under
distinct headings — are defensible and were not chosen on evidence. The repair
merged without independent review, so the choice is live in `main`.

*Resolution:* confirm the current shape, or reopen and restructure.

## ISS-2

**Markdown linting cannot detect content splices** — accepted.

The linter found the splice in [#6] only because it happened to break a code
fence and a numbered list. A splice landing on clean section boundaries would
pass every check. Two of these have now occurred, both from merges.

*Mitigation:* `AGENTS.md` asks for a read of the rendered diff before opening a
pull request. There is no mechanical guard.

## ISS-3

**The linter version is unpinned for local runs** — accepted.

`npx markdownlint-cli2` fetches the current release, so a local run and a CI
run can use different versions and disagree. CI pins only to an action major
version, which takes minor and patch updates automatically.

*Resolution, if it becomes a problem:* add a `package.json` and lockfile, at
the cost of introducing npm dependency management to a repository with none;
or pin the actions to full commit SHAs.

## ISS-4

**External URLs are not link-checked** — accepted.

The link check runs with `--offline`, covering relative paths and in-page
anchors only. External links can rot without CI noticing.

This is deliberate: external checks fail for rate limiting and transient
outages that say nothing about this repository, and a lint job that cries wolf
gets ignored. The documents most exposed are `shared/okf-bundle-setup.md`,
which cites an external specification, and `docs/CHANGELOG.md`.

## ISS-5

**Two files name the standalone documents under `shared/`** — open.

`README.md` names them in prose and `shared/README.md` lists them under
Contents. [#5] removed the duplicated *index*, not every mention, so the two
can still drift. `AGENTS.md` asks that they be kept in sync.

*Resolution:* accept the small duplication, or have the root link only to
`shared/README.md`.

## ISS-6

**`shared/okf-bundle-setup.md` targets a pre-1.0 specification** — open.

The document targets Open Knowledge Format v0.2. The format is young, and a
v0.3 or v1.0 may rename fields as v0.2 renamed two of v0.1's. The document
states the version it targets so the drift is visible, but nothing detects it.

*Resolution:* re-check the specification periodically; see [ROADMAP](ROADMAP.md).

## ISS-7

**Merged pull request branches remain on the remote** — open.

Four `agent/*` branches and two `copilot/*` branches survive their merges.

*Resolution:* delete them, and enable *Automatically delete head branches* in
repository settings.

[#2]: https://github.com/JefTek/ai-agent-info/pull/2
[#5]: https://github.com/JefTek/ai-agent-info/pull/5
[#6]: https://github.com/JefTek/ai-agent-info/pull/6
