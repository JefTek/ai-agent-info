# Setting Up an OKF Bundle

Follow this document to create a new Open Knowledge Format (OKF) bundle
from scratch, or to bring an existing folder of notes into conformance.

OKF is an open, vendor-neutral format for representing knowledge — the
metadata, context, and curated insight that surrounds data and systems —
as a directory of markdown files with YAML frontmatter. There is no
schema registry, no central authority, no SDK, and no required runtime.

**Target version: OKF v0.2.** The v0.2 specification is authoritative for
everything below. The Google Cloud launch blog post describes v0.1;
v0.2 supersedes it and renames two v0.1 fields (see
[Migrating from v0.1](#migrating-from-v01)). Prefer the spec over the
blog post when they disagree.

- Specification: <https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md>
- Background: <https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing>
- Reference bundles: <https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf/bundles>

## Why an agent should use OKF

Organizations scatter context across metadata catalogs with proprietary
APIs, wikis, shared drives, code comments, and individual engineers'
heads. An agent answering a question has to reassemble it from mutually
incompatible surfaces. OKF collapses that into files an agent can read
directly.

- **No SDK between the agent and the content.** A concept can be read
  with `cat` and ingested verbatim into context. No query language, no
  auth flow, no vendor client.
- **Both a producer and a consumer format.** The same bundle an agent
  reads is one an agent can write. Agents maintain cross-references and
  touch many files per pass without getting bored, so a corpus improves
  as it is used.
- **Version controlled like code.** Bundles live in git, so pull
  requests, diffs, blame, and review apply to knowledge curation.
- **Portable and lock-in free.** A bundle is a directory: ship it as a
  tarball, host it in any repo, mount it from any filesystem.
- **Interoperable across producers and consumers.** The format is the
  contract, so tooling on each end is independently swappable — bundles
  written by one team's pipeline are consumable by another team's agent
  without translation.
- **Trust is queryable, not implied.** v0.2 puts provenance, trust
  tier, and freshness in frontmatter, so an agent can decide how much
  to rely on a concept before acting on it.
- **Progressive disclosure.** `index.md` files let an agent navigate one
  level at a time instead of loading the whole bundle into context.
- **Graph-shaped.** Concepts cross-link with ordinary markdown links,
  expressing relationships richer than the directory tree.

Use OKF when knowledge needs to outlive one tool, be reviewed by humans,
and be consumed by agents that were not built alongside it. Reach for a
purpose-built catalog instead when the requirement is enforcement,
access control, or transactional metadata — OKF is advisory by design.

## Before starting

Collect these before writing files. Ask the requester rather than
guessing.

1. **Bundle scope** — the domain being documented and its boundary
   (one dataset, one service, one business area).
2. **Bundle path** — where the bundle lives, e.g. `bundles/<name>/`.
3. **Concept types** — the kinds of things being documented
   (`BigQuery Table`, `Metric`, `API Endpoint`, `Playbook`,
   `Reference`, …). Types are producer-defined; there is no registry.
4. **Sources of truth** — the systems, docs, and URLs the content will
   be derived from and cited against.
5. **Actor identity** — what to record in `generated.by` for content
   produced during this task (see [Actor convention](#actor-convention)).
6. **Verification expectations** — whether a human reviews before the
   bundle is considered trustworthy.

Do not invent facts to fill a bundle. An empty section is better than a
fabricated schema, metric definition, or citation.

## Bundle layout

A bundle is a directory tree. The structure is domain-independent —
organize concepts however suits the knowledge being captured.

```text
bundles/<name>/
  index.md                  # Optional. Directory listing for progressive disclosure.
  log.md                    # Optional. Chronological history of updates.
  <concept>.md              # A concept at the bundle root.
  tables/
    index.md
    orders.md
    customers.md
  metrics/
    index.md
    weekly-active-users.md
  computations/             # Attested Computations, if used.
    index.md
    revenue.md
  references/               # Mirrored external material, executors, attesters.
    skills/run-on-bq.md
    attesters/revenue.py
```

`index.md` and `log.md` are reserved filenames at every level and MUST
NOT be used for concept documents. Every other `.md` file is a concept.

A bundle may be distributed as a git repository (recommended, for
history and diffs), a tarball or zip, or a subdirectory of a larger repo.

## Concept documents

Every concept is one UTF-8 markdown file: a YAML frontmatter block
delimited by `---`, followed by a markdown body.

### Frontmatter

```yaml
---
type: <Type name>                   # REQUIRED — the only always-required key
title: <Display name>               # Recommended
description: <One-line summary>     # Recommended
resource: <Canonical URI of the underlying asset>   # Recommended, when one exists
tags: [<tag>, <tag>]                # Recommended
---
```

- `type` is a short string used by consumers for routing, filtering, and
  presentation. Pick descriptive, self-explanatory values. Consumers
  must tolerate unknown types.
- `resource` is absent for concepts describing abstract ideas rather
  than physical assets.
- Producers MAY add any additional keys. Consumers MUST NOT reject a
  document for unrecognized fields, and SHOULD preserve them when
  round-tripping.

### Body

Standard markdown. Favor structural markdown — headings, lists, tables,
fenced code blocks — over freeform prose, since structure helps both
human reading and agent retrieval.

No body sections are required. These headings carry conventional
meaning and SHOULD be used when applicable:

| Heading         | Purpose                                             |
| --------------- | --------------------------------------------------- |
| `# Schema`      | Structured description of an asset's columns/fields |
| `# Examples`    | Concrete usage examples, usually fenced code blocks |
| `# Computation` | The sanctioned computation of an Attested Computation |

### Example

```markdown
---
type: BigQuery Table
title: Customer Orders
description: One row per completed customer order across all channels.
resource: https://console.cloud.google.com/bigquery?p=acme&d=sales&t=orders
tags: [sales, orders, revenue]
generated: { by: reference_agent/gemini-2.5-pro, at: 2026-05-28T14:30:00Z }
---

# Schema

| Column        | Type      | Description                                         |
| ------------- | --------- | --------------------------------------------------- |
| `order_id`    | STRING    | Globally unique order identifier.                   |
| `customer_id` | STRING    | Foreign key into [customers](/tables/customers.md). |
| `total_usd`   | NUMERIC   | Order total in US dollars.                          |
| `placed_at`   | TIMESTAMP | When the customer submitted the order.              |

# Joins

Joined with [customers](/tables/customers.md) on `customer_id`.
```

## Provenance, trust, and lifecycle

All of these fields are optional, but their absence carries meaning: an
unverified concept is distinguishable from a verified one and is never
rejected. Populate them whenever the information is actually known.

### `sources` — where the content came from

```yaml
sources:
  - id: ga4-schema
    resource: https://developers.google.com/analytics/bigquery/export-schema
    title: GA4 BigQuery Export schema
    author: team:ga4-docs
    usage_count: 5000
    last_modified: 2026-05-30
usage_window: { from: 2026-06-01, to: 2026-06-30 }
```

- `resource` is required within an entry. It names either a followable
  artifact (absolute URL, bundle-relative path, or a path into
  `references/`) or a scope descriptor it cannot follow, such as
  `all queries in BigQuery project X`.
- `id` is optional but SHOULD be present when the body cites the source.
- `author`, `usage_count`, and `last_modified` are optional credibility
  signals. OKF records signals, never a credibility score — scores are
  subjective, unportable, and go stale.
- `usage_window` is written once as a sibling of `sources` and frames
  every `usage_count`; a single entry MAY override it.
- Treat `usage_count` as a liveness and trend signal, not a precise
  ranking across different kinds of usage.

Attribute an individual claim with a markdown footnote whose label is a
`sources[].id`:

```markdown
The `events_` table is sharded daily as `events_YYYYMMDD`.[^ga4-schema]

[^ga4-schema]: GA4 BigQuery Export schema
```

Labels are keyed rather than positional because agents rewrite these
documents constantly — a positional index misattributes silently the
moment the list is reordered.

### `generated` and `verified` — trust

```yaml
generated: { by: reference_agent/gemini-2.5-pro, at: 2026-06-20T22:53:05Z }
verified:
  - { by: human:ahormati, at: 2026-06-25T09:00:00Z }
  - { by: process:finance-nightly, at: 2026-06-26T02:00:00Z }
```

- `generated.by` is required within `generated`; `generated.at` is an
  ISO 8601 datetime marking the last meaningful content change.
- `verified` is a list of confirmation events. Who *wrote* a concept
  need not be who *confirmed* it, which is why they are separate.
- A single verifier MAY be written as a bare `{ by, at }` mapping;
  consumers MUST treat it as a one-element list.

Consumers derive a **trust tier** from `verified`:

| Condition                                   | Tier              |
| ------------------------------------------- | ----------------- |
| No `verified` key                           | unverified        |
| `verified` by non-`human:` actors only      | machine-confirmed |
| `verified` includes a `human:<id>` actor    | human-reviewed    |

Trust tiers are advisory signals, not access control. Never record a
`verified` entry for a check that did not happen.

### `status` and `stale_after` — lifecycle

```yaml
status: stable        # draft | stable | deprecated ; absent means stable
stale_after: 2026-09-23   # absolute date; stale when today >= this date
```

`stale_after` is an absolute date rather than a relative TTL so that
staleness is a plain date comparison, independent of when the concept
was read.

## Actor convention

Identity fields (`generated.by`, `verified[].by`, `sources[].author`)
use one convention:

- `<producer>/<version>` for agents and tools —
  `reference_agent/gemini-2.5-pro`
- `human:<id>` for a person — `human:ahormati`
- `process:<id>` for an automated process — `process:finance-nightly`

Trust classification keys off the `human:` prefix, so use it only for
hand-authored or human-confirmed content.

## Cross-linking

Link concepts with standard markdown links. Two forms are supported:

- **Absolute (bundle-relative)** — begins with `/`, resolved from the
  bundle root. **Recommended**, because it survives moving a document
  within its subdirectory.

  ```markdown
  See the [customers table](/tables/customers.md) for the join key.
  ```

- **Relative** — an ordinary relative path, e.g. `./other.md`.

A link asserts a relationship; the kind of relationship is conveyed by
the surrounding prose, not by the link. Broken links are tolerated — a
link to a document that does not exist yet represents not-yet-written
knowledge, not a malformed bundle.

Path-valued fields (`resource`, `sources[].resource`, `computation`,
`executor.resource`, `attester.resource`) accept an absolute URL, a
bundle-relative path beginning with `/`, or a relative path.

By convention, a `references/` subdirectory mirrors external material,
run instructions, or code as first-class concepts inside the bundle.

## Index files

`index.md` MAY appear in any directory, including the bundle root, and
enumerates that directory's contents so a reader can see what exists
before opening documents.

Index files contain **no frontmatter**, with one exception: a
bundle-root `index.md` MAY carry `okf_version`.

```markdown
# Subdirectories

* [tables](tables/index.md) - BigQuery tables the bundle grounds against.
* [metrics](metrics/index.md) - Business definitions of headline numbers.

# Concepts

* [Orders](orders.md) - One row per completed customer order.
```

Entries SHOULD reuse the `description` from the linked concept's
frontmatter. Regenerate index files whenever concepts are added,
removed, or renamed.

## Log files

`log.md` MAY appear at any level to record the history of changes to
that scope, newest first, with ISO 8601 `YYYY-MM-DD` date headings.

```markdown
# Bundle history

## 2026-05-22

* **Update**: Added a BigQuery table reference for [Customer Metrics](/tables/customer-metrics.md).
* **Creation**: Established the [Dataplex Playbook](/playbooks/dataplex.md).

## 2026-05-15

* **Initialization**: Created foundational directory structure.
```

The leading bold word (`**Update**`, `**Creation**`, `**Deprecation**`)
is a convention, not a requirement. Unlike `index.md`, the spec does not
forbid frontmatter on `log.md`; the reference bundles give theirs a
`type: Log` block. Either form is acceptable — do not strip an existing
one.

## Attested Computations (optional)

Use `type: Attested Computation` when a concept carries a *sanctioned
way to compute a value*, so a consumer can confirm the blessed
computation ran rather than an improvised one. Provenance answers "where
did this claim come from"; attestation answers "was this number produced
the way we said it must be." OKF records the computation and the means
to check it; it executes nothing itself.

Each computation is its own concept — trust state is per computation,
`runtime` defines what `parameters` mean, and one computation can back
many consumers. Concepts that need the value link to it normally.

```markdown
---
type: Attested Computation
title: Revenue for fiscal year
description: Recognized revenue for a fiscal year, per Finance's definition.
status: stable
runtime: bigquery
parameters:
  - { name: year, type: integer, required: true }
executor:
  resource: references/skills/run-on-bq.md
  receipt: [job_id, executed_sql, result]
attester:
  resource: references/attesters/revenue.py
generated: { by: reference_agent/gemini-2.5-pro, at: 2026-06-20T22:53:05Z }
verified: { by: human:ahormati, at: 2026-06-25T09:00:00Z }
stale_after: 2026-09-23
sources:
  - id: rev-policy
    resource: https://wiki.acme/finance/revenue-recognition
    title: Revenue recognition policy
---

# Computation

    SELECT SUM(amount) AS revenue
    FROM finance.recognized_revenue
    WHERE fiscal_year = @year

The computation binds only the declared `parameters`, per the recognition
policy.[^rev-policy]

[^rev-policy]: Revenue recognition policy
```

Field notes:

- `runtime` is required for this type — `bigquery`, `postgres`, `dbt`,
  `python`, `Looker`, and so on. It determines how `parameters` bind.
- `parameters` is a list of `{ name, type, required }` — the only holes
  an agent may fill.
- `computation` optionally points at a file instead of an inline
  `# Computation` fence. Use a file for long or generated computations;
  use the inline fence for short ones reviewed alongside the contract.
- `executor.resource` names run instructions or code;
  `executor.receipt` declares the fields a run must return.
- `attester.resource` names deterministic, no-LLM code that inspects a
  receipt and returns a verdict, meant to run consumer-side.

An agent consuming one of these MAY supply only *values* for declared
parameters. It MUST NOT author or edit the computation. Receipts and
verdicts are runtime artifacts and are **not** stored in the bundle.

`verified` and attestation are distinct and both exist: `verified`
confirms the definition still matches policy (doc-level, slow, stored);
attestation confirms one run produced its value the sanctioned way
(per-call, runtime, not stored).

## Setup procedure

Follow these steps in order.

1. **Confirm scope and destination** with the requester. If this repo's
   [Git workflow](git-workflow.md) applies, create a task branch first.
2. **Create the bundle directory** at the agreed path.
3. **Draft the type vocabulary** — the small set of `type` values this
   bundle will use. Write it down before authoring concepts so values
   stay consistent.
4. **Author concepts**, one markdown file per concept, starting with
   the assets or ideas that others will link to. Give every file a
   `type`, and add `title`, `description`, `resource`, and `tags`
   whenever they are known.
5. **Record provenance as you go.** Add `sources` entries for the
   material each concept was derived from, and footnote individual
   claims by `sources[].id`. Do not cite a source that was not read.
6. **Stamp trust and lifecycle.** Set `generated: { by, at }` on every
   concept produced during this task, using the actor convention. Set
   `status` and `stale_after` where the requester has a policy. Leave
   `verified` absent until a real verification happens.
7. **Cross-link** related concepts using bundle-absolute paths.
8. **Add Attested Computations** for any sanctioned numeric definition,
   with their executors and attesters under `references/`.
9. **Generate `index.md`** for the bundle root and each subdirectory,
   reusing concept descriptions.
10. **Create `log.md`** at the bundle root with an initialization entry
    dated today.
11. **Declare the version** — add `okf_version: "0.2"` to the
    bundle-root `index.md` frontmatter (the only place frontmatter is
    allowed in an index file).
12. **Run the conformance check** below and fix what it reports.
13. **Report back** the bundle path, concept count, type vocabulary,
    and anything left unverified or unwritten.

## Conformance checklist

A bundle is conformant with OKF v0.2 when:

- [ ] Every non-reserved `.md` file has a parseable YAML frontmatter
      block.
- [ ] Every frontmatter block has a non-empty `type`.
- [ ] Every `index.md` present follows the index structure and carries
      no frontmatter, except an optional `okf_version` at the bundle
      root.
- [ ] Every `log.md` present uses `YYYY-MM-DD` date headings, newest
      first.

When the optional families are used, also confirm:

- [ ] Every `sources` entry has a `resource`.
- [ ] Every footnote label used for attribution matches a
      `sources[].id`.
- [ ] Every `generated` block has a `by`, in the actor convention.
- [ ] Every `verified` entry corresponds to a verification that
      actually occurred.
- [ ] Every `stale_after` is an absolute `YYYY-MM-DD` date.
- [ ] Every `Attested Computation` concept has a `runtime` and exactly
      one computation source — an inline `# Computation` fence or a
      `computation` path, not both.

A conformant consumer MUST NOT reject a bundle for missing optional
fields, unknown `type` values, unknown extra frontmatter keys, broken
cross-links, or missing `index.md` files. Do not "fix" any of these by
deleting content.

### Local check

Read-only; reports problems without modifying the bundle.

```bash
python3 - <<'PY' bundles/<name>
import pathlib, sys, yaml   # pip install pyyaml

root = pathlib.Path(sys.argv[1])
reserved = {"index.md", "log.md"}
problems = []

for path in sorted(root.rglob("*.md")):
    rel = path.relative_to(root)
    text = path.read_text(encoding="utf-8")
    has_fm = text.startswith("---\n")

    if path.name in reserved:
        # index.md carries no frontmatter, except okf_version at the root.
        if path.name == "index.md" and has_fm and rel.parent != pathlib.Path("."):
            problems.append(f"{rel}: index.md must not have frontmatter")
        continue

    if not has_fm:
        problems.append(f"{rel}: missing frontmatter block")
        continue

    _, _, rest = text.partition("---\n")
    block, sep, _ = rest.partition("\n---")
    if not sep:
        problems.append(f"{rel}: unterminated frontmatter block")
        continue

    try:
        fm = yaml.safe_load(block) or {}
    except yaml.YAMLError as exc:
        problems.append(f"{rel}: unparseable YAML ({exc.__class__.__name__})")
        continue

    if not fm.get("type"):
        problems.append(f"{rel}: missing required 'type'")
    if fm.get("type") == "Attested Computation" and not fm.get("runtime"):
        problems.append(f"{rel}: Attested Computation missing 'runtime'")
    for i, src in enumerate(fm.get("sources") or []):
        if isinstance(src, dict) and not src.get("resource"):
            problems.append(f"{rel}: sources[{i}] missing 'resource'")

print("\n".join(problems) if problems else "OK: bundle is v0.2 conformant")
PY
```

The check covers structural conformance only. It cannot tell whether the
content is true, whether a citation supports its claim, or whether a
`verified` entry reflects a real review — those stay human
responsibilities.

## Migrating from v0.1

v0.2 is otherwise backward compatible, but two v0.1 fields are renamed
or retired:

| v0.1                     | v0.2                                          |
| ------------------------ | --------------------------------------------- |
| `timestamp: <datetime>`  | `generated: { by: <actor>, at: <datetime> }`  |
| Body `# Citations` list  | `sources` frontmatter + keyed footnotes       |

Everything else in v0.1 carries forward unchanged. When updating an
existing bundle, convert both fields, then add the new optional
families (`verified`, `status`, `stale_after`, credibility signals)
where the information is genuinely known.

## Guardrails

- Do not fabricate schemas, metric definitions, citations, credibility
  signals, or verification events. Absent frontmatter is meaningful and
  is always preferable to invented frontmatter.
- Do not put credentials, tokens, customer data, or internal-only URLs
  that leak secrets into a bundle. Bundles are designed to be shared.
- Do not record a `human:` actor for work an agent did.
- Do not delete or rewrite concepts authored by others to satisfy the
  conformance check; report the discrepancy instead.
- Do not add a schema registry, required tooling, or a custom parser.
  If a consumer needs something the format lacks, add optional
  frontmatter keys — consumers must tolerate them.
- Prefer extending an existing bundle over creating a parallel one for
  the same domain.
