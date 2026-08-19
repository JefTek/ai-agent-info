# ai-agent-info

Shared guidance for AI agents working in repositories and workspaces.

## Shared guidance

- [Git workflow instructions](shared/git-workflow.md)
A practical, workspace-neutral library of notes, tips, tricks, skills, and
agent patterns for Claude and GitHub Copilot.

## Library

| Area | Contents |
| --- | --- |
| [Claude AI](claude-ai/README.md) | Context management, prompt patterns, skills, and agent roles |
| [GitHub Copilot](copilot/README.md) | Repository guidance, coding workflows, skills, and agent roles |
| [Shared guidance](shared/README.md) | Practices that apply to either assistant |

## Use in a workspace

1. Start with the relevant assistant guide above.
2. Copy an applicable prompt or agent role into the workspace's instruction
   file or chat.
3. Replace bracketed placeholders with project-specific details.
4. Keep instructions short, specific, and verified against the repository.

Each entry is deliberately tool-agnostic where possible. Adapt commands,
paths, and guardrails to the workspace instead of assuming examples are safe
for every project.

## Contributing

See [the contribution guide](docs/contributing.md) for the entry format and
guidelines for adding a reusable item.
