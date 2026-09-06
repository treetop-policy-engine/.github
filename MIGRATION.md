# Coordinated breaking release: declared label targets

Released 2026-09-06 after coordinated review and verification. This document is
the organization announcement, breaking migration guide, and upgrade order.

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

| Repository | Release | Review | Required update |
| --- | --- | --- | --- |
| Core | [0.1.0](https://crates.io/crates/treetop-core/0.1.0) | [PR #60](https://github.com/treetop-policy-engine/treetop-core/pull/60) | Implement `target` and `derive`, use validated targets, remove deprecated listing aliases |
| Bundle | [0.1.0](https://github.com/treetop-policy-engine/treetop-bundle/releases/tag/v0.1.0) | [PR #10](https://github.com/treetop-policy-engine/treetop-bundle/pull/10) | Declare targets, adopt format 2, rebuild and re-sign archives |
| REST | [0.1.0](https://github.com/treetop-policy-engine/treetop-rest/releases/tag/v0.1.0) | [PR #73](https://github.com/treetop-policy-engine/treetop-rest/pull/73) | Deploy new configurations and archives; adopt current endpoints and metadata |
| Rust SDK | [0.1.0](https://crates.io/crates/treetop-client/0.1.0) | [PR #15](https://github.com/treetop-policy-engine/treetop-client/pull/15) | Remove deprecated constructors/health alias; use required metadata and separate schema revisions |
| CLI | [0.1.0](https://github.com/treetop-policy-engine/treetop-cli/releases/tag/v0.1.0) | [PR #9](https://github.com/treetop-policy-engine/treetop-cli/pull/9) | Upgrade the SDK/server pair and remove legacy response assumptions |
| Python | [0.1.0](https://pypi.org/project/treetop-client/0.1.0/) | [PR #13](https://github.com/treetop-policy-engine/treetop-client-python/pull/13) | Use uniform batch methods, canonical decisions, required metadata, and complete response correspondence |
| Go | [0.3.0](https://pkg.go.dev/github.com/treetop-policy-engine/treetop-client-go@v0.3.0) | [PR #4](https://github.com/treetop-policy-engine/treetop-client-go/pull/4) | Use current metadata, `Live`/`Ready`, and distinct schema revision types |
| Frontend | [0.1.0](https://github.com/treetop-policy-engine/treetop-frontend/releases/tag/v0.1.0) | [PR #4](https://github.com/treetop-policy-engine/treetop-frontend/pull/4) | Use exact target fields and strict generated/current responses; rebuild demos |
| Bundle Action | [2.0.0 / v2](https://github.com/treetop-policy-engine/treetop-bundle-action/releases/tag/v2.0.0) | [PR #8](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/8) | Require CLI 0.1.0, format 2, and exact checksum asset names |
| Organization | Documentation | This announcement | Adopt the shared early-release correctness policy and rollout guidance |

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

## Upgrade and release order

Every consumer uses published prerequisites. Bundle requires Core 0.1.0 from
crates.io; REST requires Core and Bundle 0.1.0; CLI requires Rust SDK 0.1.0.
Temporary Cargo Git patches are removed, with registry checksums in the application
and fuzz lockfiles. Rust SDK, Python, Go, and CLI integration checks pin the REST
release image by digest; the frontend pins its exact released source commit.
Action v2 checks both the released Bundle source and the actual published CLI
assets on every native platform.

Upgrade in this order:

1. Core 0.1.0 for embedded Rust applications and custom labeler implementations.
2. Bundle 0.1.0 and its `treetop-bundle` validation CLI, updated format 2 sources,
   and rebuilt/re-signed archives.
3. REST 0.1.0 with the new configuration and archive contract.
4. Rust SDK/Python 0.1.0 and Go 0.3.0 against the released REST contract.
5. `treetop-cli`/Workbench 0.1.0 and Bundle Action v2 after their prerequisites are
   available.

The coordinated PRs are squash-merged in dependency order. Release tags point to
verified main commits; package archives and runtime artifacts are checked before
downstream publication. Future releases must preserve substantive migration notes,
keep immutable tags fixed, and pass performance/security checks.

## Performance considerations

Declared Cedar types are parsed and validated during construction/reload, not
on each evaluation. Core's new cold target parsing cost is reported separately
from registration and evaluation; benchmark boundaries must stay explicit.
Go's required-field validation currently adds about 4–5% to local response-parse
medians while retaining allocation counts. Python reports no CodSpeed regressions,
but earlier comparisons included an environment warning, so the reported
improvement is not treated as a controlled performance measurement. Each implementation PR
contains its verification evidence and limitations.
