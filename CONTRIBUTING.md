# Contributing

Add reusable guidance that works across multiple workspaces. Keep entries
short, actionable, and safe by default.

## Where to add content

| Area | File | For |
| --- | --- | --- |
| Claude AI | `claude-ai/README.md` | Claude-specific guidance |
| GitHub Copilot | `copilot/README.md` | Copilot-specific guidance |
| Shared | `shared/README.md` | Assistant-neutral practices |

The assistant guides are organized into the sections **Notes**, **Tips and
tricks**, **Skills**, and **Agent roles**. Add to an existing section rather
than inventing a new one.

`shared/` is organized differently: `shared/README.md` carries the directory's
purpose, use cases, and index, plus short practices that fit inline
(**Reliable workflow**, **Prompt pattern**, **Guardrails**). Longer procedures
live in their own file alongside it.

## Entry format

Short entries go inline, in the section that matches their kind:

- **Notes** capture durable observations and limitations.
- **Tips and tricks** include a short explanation and a copyable prompt.
- **Skills** state when to use a capability and provide a prompt starter.
- **Agent roles** state the agent's goal, scope, and expected output.

## Standalone documents

Add a new file when a procedure is too long to scan inline — roughly, when it
needs its own sections, or when an agent would be pointed at the whole file
rather than one section of it. `shared/git-workflow.md` and
`shared/okf-bundle-setup.md` are the current examples.

A standalone document should:

- Open with a sentence saying what the document is for and when to follow it.
- State any external specification or version it targets, so drift is visible.
- Be written to be handed to an agent verbatim, with bracketed placeholders
  for project-specific details.
- End with guardrails covering what not to do.
- Be listed in its directory's **Contents** section with a one-line
  description.

Use sentence case for headings, except where a proper noun or a term from an
external specification requires otherwise.

## Linting

Markdown is linted with [markdownlint][ml]. Run it before opening a pull
request; CI runs the same check.

```bash
npx markdownlint-cli2
```

Rules live in `.markdownlint-cli2.jsonc`. The ones worth knowing: prose wraps
at 80 characters, while tables, code blocks, and headings are exempt.

[ml]: https://github.com/DavidAnson/markdownlint-cli2

## What to avoid

Avoid secrets, personal data, vendor claims that cannot be verified, and
commands that mutate or publish a workspace without an explicit guardrail.
Use placeholders such as `[feature]` and `[path]` so entries remain reusable.
