# Claude AI

## Notes

- Give Claude the goal, relevant constraints, and the files or excerpts needed
  to make a decision. Broad requests without repository context tend to produce
  broad changes.
- State whether an answer should be a plan, an explanation, a patch, or a
  review. This makes the desired output explicit.
- Treat generated output as a draft: run the project's existing tests and
  inspect the resulting diff before accepting it.

## Tips and tricks

### Progressive context

Start with the task and directory layout. Add the exact implementation and
test files only after identifying the relevant area. This preserves context
for the details that affect a change.

### Constrained change prompt

```text
Make the smallest complete change for [goal].
Inspect [paths] first. Preserve [compatibility/security/style constraints].
Add or update focused tests using the existing test framework.
Show the changed files and the validation commands you ran.
```

### Review prompt

```text
Review this diff for correctness, regressions, security issues, and missing
tests. Report only actionable findings, with file and line references.
```

## Skills

| Skill | Use it when | Prompt starter |
| --- | --- | --- |
| Codebase discovery | The owning code is unknown | “Trace how [feature] reaches [component], naming the files involved.” |
| Focused implementation | The scope is clear | “Implement [behavior] only in [area]; do not refactor unrelated code.” |
| Test design | A regression needs coverage | “Identify the smallest existing test location and add cases for [behavior].” |
| Code review | A diff is ready | “Find high-confidence defects in this diff; ignore cosmetic suggestions.” |

## Agent roles

### Repository explorer

```text
Inspect the repository without modifying it. Map the files responsible for
[feature], its tests, and the documented validation commands. Recommend the
smallest change; report uncertainty explicitly.
```

### Implementation agent

```text
Implement [requirement] in [scope]. First inspect nearby conventions and
tests. Make no unrelated changes. Validate with the most targeted existing
checks and summarize the results.
```

### Verification agent

```text
Independently inspect the completed change for missed edge cases, unsafe input
handling, and compatibility risks. Do not edit files. Cite exact evidence for
each finding.
```
