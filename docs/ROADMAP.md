# Roadmap

Candidate work, in rough priority order. Nothing here is a commitment or a
dated plan: this repository has no release cycle, and items are picked up when
someone needs them.

An item earns a place here by being concrete enough to act on. Vague ambitions
("more content") do not belong; a named gap does.

## Next

Small, well-understood work that unblocks or de-risks something.

- **Settle the shape of `shared/git-workflow.md`.** The repair in [#6] chose
  the longer of two interleaved versions without independent review. Confirm
  or restructure, then close [ISS-1](ISSUES.md#iss-1).
- **Clear the merged branches** and enable *Automatically delete head
  branches*, closing [ISS-7](ISSUES.md#iss-7).
- **Reconcile `claude-ai/README.md` and `copilot/README.md`.** They carry the
  same four sections and near-identical advice in different words. Either
  factor the shared half into `shared/` and leave only assistant-specific
  material, or state plainly that the duplication is intentional so the next
  reader stops wondering.

## Later

Worth doing, not yet urgent.

- **Re-check the Open Knowledge Format specification** and update
  `shared/okf-bundle-setup.md` if a version past v0.2 lands, closing
  [ISS-6](ISSUES.md#iss-6). The document's conformance checklist and validator
  are the parts most likely to need changes.
- **Grow `shared/` where a procedure is repeated by hand.** A standalone
  document earns its place when an agent would be pointed at the whole file.
  Candidates seen so far: reviewing a pull request, and writing a repository
  instruction file (`AGENTS.md` and friends) for another project.
- **Decide whether to pin tooling.** A `package.json` and lockfile would make
  local and CI linting agree exactly, closing [ISS-3](ISSUES.md#iss-3), at the
  cost of introducing npm dependency management to a repository with none.
  Worth doing only if a version disagreement actually bites.

## Not planned

Recorded so the question is not reopened without new information.

- **A documentation site.** The library is read on GitHub and pasted into
  workspaces. A rendered site would add build tooling and a deploy step
  without changing how the content is used.
- **Versioned releases of the guidance.** Consumers copy entries rather than
  depend on a version, and the changelog already carries the history.
- **Mechanical detection of content splices.** Two merges have produced them,
  but detecting a splice that lands on clean section boundaries means
  understanding the prose, which a linter cannot do. Tracked as an accepted
  limitation in [ISS-2](ISSUES.md#iss-2) instead.

[#6]: https://github.com/JefTek/ai-agent-info/pull/6
