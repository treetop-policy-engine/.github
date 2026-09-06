# Coordinated breaking release: declared label targets

Status: proposed release, all changes held for user approval. No merge, version
tag, registry publication, or release announcement is authorized by a green PR.
This document is the proposed organization announcement and rollout guide.

Early Treetop releases prioritize correctness over compatibility across every
repository. Breaking changes must be intentional, tested, announced, and supplied
with concrete migration steps. Remove obsolete aliases and defaults instead of
preserving ambiguous behavior behind compatibility layers.

## One owner per declared tuple

Label ownership is `(fully qualified Cedar resource type, attribute name)`.
`App::Host.labels` and `Other::Host.labels` are independent. Two rules cannot own
`App::Host.labels`; combine their patterns under one declaration or rename an
output. A root type such as `Host` is exact, not a wildcard or an alias for every
namespaced Host.

```json
{
  "target": {"resource_type": "App::Host", "attribute": "labels"},
  "field": "name",
  "patterns": [{"name": "prod", "regex": "^prod"}]
}
```

Replace old `kind` and `output` fields with this target. Labelers declare a
validated `LabelTarget` and implement read-only `derive`; `applies_to` and `output`
are removed. Receiver-style `LabelerApply::apply` owns mutation. Core freezes
ownership at registration, rejects duplicate tuples and reserved `id`, matches
exact resource types, and clears every owned attribute on that type before any
rule derives its output. Repeated applications replace or remove owned values.

Scoping includes sanitation. An `App::Host` owner does not clear attributes on
`Other::Host`. Policies must constrain the actual resource type before trusting a
derived attribute. Bundle delegates runtime application to Core; clients and the
workbench consume the server contract and do not implement policy evaluation.

## Required consumer updates

| Repository | Planned version | Review | Required update |
| --- | --- | --- | --- |
| Core | 0.1.0 | [PR #60](https://github.com/treetop-policy-engine/treetop-core/pull/60) | Implement `target` and `derive`, use validated targets, remove deprecated listing aliases |
| Bundle | 0.1.0 | [PR #10](https://github.com/treetop-policy-engine/treetop-bundle/pull/10) | Declare targets, adopt format 2, rebuild and re-sign archives |
| REST | 0.1.0 | [PR #73](https://github.com/treetop-policy-engine/treetop-rest/pull/73) | Deploy new configurations and archives; adopt current endpoints and metadata |
| Rust SDK | 0.1.0 | [PR #15](https://github.com/treetop-policy-engine/treetop-client/pull/15) | Remove deprecated constructors/health alias; use required metadata and separate schema revisions |
| CLI | 0.1.0 | [PR #9](https://github.com/treetop-policy-engine/treetop-cli/pull/9) | Upgrade the SDK/server pair and remove legacy response assumptions |
| Python | 0.1.0 | [PR #13](https://github.com/treetop-policy-engine/treetop-client-python/pull/13) | Use uniform batch methods, canonical decisions, required metadata, and complete response correspondence |
| Go | 0.3.0 | [PR #4](https://github.com/treetop-policy-engine/treetop-client-go/pull/4) | Use current metadata, `Live`/`Ready`, and distinct schema revision types |
| Frontend | 0.1.0 | [PR #4](https://github.com/treetop-policy-engine/treetop-frontend/pull/4) | Use exact target fields and strict generated/current responses; rebuild demos |
| Bundle Action | 2.0.0 / v2 | [PR #8](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/8) | Require CLI 0.1.0, format 2, and exact checksum asset names |
| Organization | Documentation | This proposed announcement | Adopt the shared early-release correctness policy and rollout guidance |

Bundle and module manifests now require `format_version = 2`. Old manifests,
archives, signatures over old archives, and old label syntax are not migrated
implicitly. Update sources, rebuild, review diagnostics, and re-sign new bytes.
Failed reloads leave the previous complete authorization generation active.

Use `/livez`, `/readyz`, and `/openapi.json`; the legacy health and OpenAPI aliases
are removed. Policy versions require `hash`, `loaded_at`, nullable `label_set`, and
unsigned 64-bit `generation`. Generation is local to an engine instance. Schema
revisions are separate hash/timestamp objects. Status includes schema metadata,
request limits, and context capabilities. An explicit zero limit never means
unlimited. JavaScript rejects integers it cannot represent exactly.

Use authorization evaluation as the source of truth. Candidate policy listings
do not execute Cedar conditions and can include forbids; they are never proof of
permission. Clients reject inconsistent batch counts, positions, IDs, versions,
and decision details. Parsing errors must never become allow decisions.

## Review and release order

The review set uses immutable source pins so every dependent PR can be built and
tested before prerequisites are published. These pins are review dependencies,
not authorization to merge or publish. All repositories remain held until the
user approves the concrete PR set.

After approval:

1. Merge and release Core 0.1.0 from its exact green main commit.
2. Switch Bundle's candidate Cargo patch to the published Core release, refresh
   locked verification, then merge and release Bundle 0.1.0 and its CLI assets.
3. Switch REST's Core/Bundle patches to registry releases, refresh checks, then
   merge and release REST 0.1.0.
4. Release Rust SDK 0.1.0, Python 0.1.0, and Go 0.3.0 against the released REST
   contract. Switch CLI's SDK patch to its registry release and reverify it.
5. Release CLI/frontend 0.1.0 and action v2 after their prerequisites exist. Replace
   candidate build pins with immutable released references and refresh all checks.
6. Publish the approved organization announcement with actual release links.

Squash PRs into main. Preserve substantive PR descriptions in squash commit
bodies, omit verification-only sections, and tag only the exact verified main
commits according to each repository's release process. Do not bypass failures,
move immutable release tags, or weaken performance/security checks to finish.

## Performance considerations

Declared Cedar types are parsed and validated during construction/reload, not
on each evaluation. Core's new cold target parsing cost is reported separately
from registration and evaluation; benchmark boundaries must stay explicit.
Go's required-field validation currently adds about 4–5% to local response-parse
medians while retaining allocation counts. Python reports no CodSpeed regressions,
but earlier comparisons included an environment warning, so the reported
improvement is not treated as a controlled performance measurement. Each implementation PR
contains its verification evidence and limitations.
