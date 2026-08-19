# Contributing

Add reusable guidance that works across multiple workspaces. Keep entries
short, actionable, and safe by default.

## Where to add content

- `claude-ai/README.md` for Claude-specific guidance.
- `copilot/README.md` for GitHub Copilot-specific guidance.
- `shared/README.md` for assistant-neutral practices.

Use the existing sections: **Notes**, **Tips and tricks**, **Skills**, and
**Agent roles**. Add a new file only when a section can no longer remain easy
to scan.

## Entry format

- **Notes** capture durable observations and limitations.
- **Tips and tricks** include a short explanation and a copyable prompt.
- **Skills** state when to use a capability and provide a prompt starter.
- **Agent roles** state the agent's goal, scope, and expected output.

Avoid secrets, personal data, vendor claims that cannot be verified, and
commands that mutate or publish a workspace without an explicit guardrail.
Use placeholders such as `[feature]` and `[path]` so entries remain reusable.
