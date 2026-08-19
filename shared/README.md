# Shared agent guidance

Guidance in this directory applies across agents and workspaces.

## Contents

- [Git workflow instructions](git-workflow.md)
# Shared guidance

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
