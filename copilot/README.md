# GitHub Copilot

## Notes

- Repository instruction files are part of the prompt. Keep them current,
  scoped to the workspace, and focused on durable conventions.
- Ask Copilot to inspect existing patterns before generating code; the
  repository is the source of truth for style and supported tooling.
- Review every proposed edit, especially generated configuration, dependency,
  shell, and authentication changes.

## Tips and tricks

### Workspace-aware task prompt

```text
Work in this repository. Before editing, inspect the implementation and tests
for [feature]. Follow their conventions, make the smallest complete change,
and run the targeted existing validation command.
```

### Issue-to-plan prompt

```text
Read this issue: [issue text]. Identify the affected files, edge cases,
existing tests to update, and a minimal implementation plan. Do not edit yet.
```

### Explain-before-edit prompt

```text
Explain the current behavior in [path or symbol], including callers and tests.
Then propose the smallest safe modification for [goal].
```

## Skills

| Skill | Use it when | Prompt starter |
| --- | --- | --- |
| Code navigation | Finding an owner or call path | “Locate the implementation, callers, and tests for [symbol].” |
| Test-first repair | Fixing a reproducible defect | “Add a focused regression test for [failure], then make it pass.” |
| Refactoring | Improving known code safely | “Preserve public behavior while simplifying [scope]; prove it with existing tests.” |
| Documentation | Explaining project behavior | “Document [behavior] from the implementation; do not invent unsupported details.” |

## Agent roles

### Issue triage agent

```text
Analyze [issue]. Find the relevant code and tests, distinguish observed facts
from assumptions, and propose the smallest fix. Do not modify files.
```

### Test agent

```text
For the change in [paths], add focused tests in the existing framework. Cover
the reported case and one meaningful boundary case without changing unrelated
tests.
```

### Change reviewer

```text
Review the current diff for logic errors, missing validation, data exposure,
and regressions. Return only findings that include a concrete failure mode.
```
