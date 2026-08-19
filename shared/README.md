# Shared agent guidance

Guidance in this directory applies across agents and workspaces. It is the
assistant-neutral half of the library: practices that hold whether the work
is done by Claude, GitHub Copilot, or another coding agent.

## Purpose

Behavior that is safe and predictable in one repository should not have to be
re-derived in the next one. This directory collects the practices that
travel:

- **Neutral by default** — nothing here depends on a particular assistant,
  model, editor, or vendor feature.
- **Operational, not aspirational** — each entry says what to do at a
  specific point in a task rather than describing a philosophy.
- **Safe to hand to an agent verbatim** — entries are written to be pasted
  into an instruction file or pointed at directly, with placeholders for
  project-specific details.
- **Verifiable** — guidance that can be checked against a repository is
  preferred over guidance that cannot.

Assistant-specific material belongs in [Claude AI](../claude-ai/README.md) or
[GitHub Copilot](../copilot/README.md) instead.

## Use cases

| Situation | Start with |
| --- | --- |
| Picking up a task in an unfamiliar repository | [Reliable workflow](#reliable-workflow) |
| Framing a task so it does not need three rounds of clarification | [Prompt pattern](#prompt-pattern) |
| Making a change that will be branched, committed, or opened as a pull request | [Git workflow instructions](git-workflow.md) |
| Capturing what a dataset, service, or metric means so agents can consume it later | [Setting up an OKF bundle](okf-bundle-setup.md) |
| Deciding whether generated output is safe to accept | [Guardrails](#guardrails) |

Typical ways to apply an entry:

1. Point an agent at the file directly when a task matches it end to end.
2. Copy the relevant section into the workspace's instruction file, such as
   `AGENTS.md`, `CLAUDE.md`, or `.github/copilot-instructions.md`, so it
   applies to every task in that workspace.
3. Paste a single section into a chat when it applies to one task only.

Replace bracketed placeholders with project-specific details, and adapt
commands and paths instead of assuming an example is safe for every project.

## Contents

- [Git workflow instructions](git-workflow.md) — branching, commits,
  validation, and pull requests for agent-authored changes.
- [Setting up an OKF bundle](okf-bundle-setup.md) — producing a conformant
  Open Knowledge Format bundle so knowledge stays portable and readable by
  both humans and agents.

## Reliable workflow

1. **Discover** — Read the task, relevant instructions, implementation, and
   existing tests before proposing changes.
2. **Constrain** — Define the expected behavior, compatibility boundaries, and
   smallest affected area.
3. **Implement** — Reuse local patterns and dependencies; avoid speculative
   refactors.
4. **Validate** — Run focused existing tests first, then broader checks when
   appropriate.
5. **Review** — Inspect the final diff for accidental files, secrets, unsafe
   input handling, and unrequested behavior changes.

## Prompt pattern

```text
Goal: [observable outcome]
Scope: [files, component, or behavior]
Constraints: [compatibility, security, performance, style]
Evidence: [tests, error output, reproduction]
Done when: [acceptance criteria and validation command]
```

## Guardrails

- Do not place credentials, tokens, private data, or production secrets in a
  prompt, source file, test fixture, or commit.
- Prefer existing project tools and conventions over adding dependencies or
  bespoke automation.
- Request clarification when requirements conflict or required access is
  unavailable.
- Verify generated commands before running them, particularly commands that
  install, delete, publish, or change infrastructure.
