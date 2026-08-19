# Changelog

Notable changes to this repository. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

This repository is documentation, not software: it carries no version number
and publishes no releases, so entries are grouped by date rather than by
release. Add notable changes under **Unreleased** as they land.

## Unreleased

Nothing yet.

## 2026-08-19

### Added

- Claude and GitHub Copilot guidance library, with notes, tips, skills, and
  agent roles for each assistant, plus assistant-neutral shared practices
  ([#1]).
- Git workflow instructions for agents, covering branching, commits,
  validation, review, and pull requests ([#2]).
- `shared/okf-bundle-setup.md`, an agent-pointable guide to producing an Open
  Knowledge Format bundle, targeting OKF v0.2 ([#3]).
- Purpose and use-case sections in `shared/README.md`, turning a bare index
  into an entry point ([#4]).
- `AGENTS.md`, so the repository applies the guidance it publishes, with
  `CLAUDE.md` and `.github/copilot-instructions.md` pointing at it ([#5]).
- Apache 2.0 license reference in the README, and a `.gitignore` ([#5]).
- Markdown linting via `markdownlint-cli2`, with a config shared by local runs
  and CI, plus an offline link check over relative paths and anchors ([#6]).

### Changed

- Moved `docs/contributing.md` to `CONTRIBUTING.md`, the path the git workflow
  names and the one GitHub surfaces ([#5]).
- Reconciled `CONTRIBUTING.md` with the actual layout: it had directed
  contributors to sections `shared/README.md` has never used, and gave no
  format for standalone documents ([#5]).
- Normalized document titles to sentence case, matching the labels the indexes
  already used to link them ([#5]).
- Rewrapped prose in `shared/git-workflow.md` to the 80-column convention, and
  aligned a table in `shared/okf-bundle-setup.md` ([#6]).

### Fixed

- Repaired the `README.md` introduction, which the merge of [#2] split apart,
  leaving a second intro orphaned under a heading ([#5]).
- Removed the duplicate shared-guidance index from `README.md`, which had
  already drifted out of sync with `shared/README.md` ([#5]).
- Repaired `shared/git-workflow.md`, which the merge of [#2] left as two
  versions of the document interleaved — a code fence with no opening
  delimiter, an orphaned list item, and three sections duplicating later ones
  ([#6]). See [ISS-1](ISSUES.md#iss-1) for the open question this repair
  raises.

[#1]: https://github.com/JefTek/ai-agent-info/pull/1
[#2]: https://github.com/JefTek/ai-agent-info/pull/2
[#3]: https://github.com/JefTek/ai-agent-info/pull/3
[#4]: https://github.com/JefTek/ai-agent-info/pull/4
[#5]: https://github.com/JefTek/ai-agent-info/pull/5
[#6]: https://github.com/JefTek/ai-agent-info/pull/6
