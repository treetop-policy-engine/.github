# Core 0.3.0 coordinated releases

Core 0.3.0 was released on 2026-10-02. Bundle 0.3.0 and the coordinated downstream
releases followed on 2026-10-03. Published dependencies, signed release tags, and
immutable server pins connect the full release set.

## Published release set

| Repository | Version | Distribution and contract |
| --- | --- | --- |
| Core | 0.3.0 | [Rust crate](https://crates.io/crates/treetop-core/0.3.0); Utoipa 6 and compact immutable permit JSON |
| Bundle | 0.3.0 | [Library and native CLI](https://github.com/treetop-policy-engine/treetop-bundle/releases/tag/v0.3.0); exact Core 0.3.0 generator metadata |
| REST | 0.2.0 | [Server archives and container](https://github.com/treetop-policy-engine/treetop-rest/releases/tag/v0.2.0); exact published Core/Bundle 0.3.0 dependencies |
| Rust client | 0.2.0 | [Rust crate](https://crates.io/crates/treetop-client/0.2.0); Rust 1.93.1 minimum and immutable REST 0.2.0 integration |
| Python client | 0.1.1 | [PyPI](https://pypi.org/project/treetop-client/0.1.1/); Python 3.12 minimum and immutable REST 0.2.0 integration |
| Go client | 0.3.1 | [Go module](https://pkg.go.dev/github.com/treetop-policy-engine/treetop-client-go@v0.3.1); Go 1.25.13 minimum and immutable REST 0.2.0 integration |
| CLI | 0.2.0 | [Native archives and checksums](https://github.com/treetop-policy-engine/treetop-cli/releases/tag/v0.2.0); exact Rust client 0.2.0 dependency and Rust 1.93.1 minimum |
| Frontend | 0.2.0 | [Workbench archive and container](https://github.com/treetop-policy-engine/treetop-frontend/releases/tag/v0.2.0); generated API, demos, and live tests use the REST release commit |
| Bundle Action | 3.0.0 / v3 | [GitHub Action](https://github.com/treetop-policy-engine/treetop-bundle-action/releases/tag/v3.0.0); published Bundle CLI 0.3.0 by default |

## Migration to Core and Bundle 0.3.0

1. Rust applications composing Core OpenAPI schemas must upgrade to Utoipa 6
   and compatible integrations. `PermitPolicy.json` now uses `Arc<PolicyJson>`;
   use `to_value()` to inspect or edit a JSON tree and `Arc::new(value.into())`
   in struct literals. `PermitPolicy::new` still accepts `serde_json::Value`.
2. Rebuild and re-sign policy archives with Bundle CLI 0.3.0 before deploying
   REST 0.2.0. The exact generator tuple is Bundle 0.3.0, Core 0.3.0, and Cedar
   4.13.0. Do not edit signed archives. Manifest/signature format 2 and exact
   declared label targets are unchanged.
3. Deploy the REST 0.2.0 container or native archive. The application and fuzz
   lockfiles use published crates with registry checksums. Client CI pins the
   released container by digest; frontend generation and demos pin its source
   commit.
4. Upgrade the Rust SDK and CLI to 0.2.0 using Rust 1.93.1 or newer. Python
   0.1.1 and Go 0.3.1 retain their existing API and minimum toolchains. HTTP
   request and decision JSON are unchanged across this refresh.
5. Use Workbench 0.2.0. Development requires Node.js 22.22.2+, 24.15.0+, or 26+;
   Node.js 23 and 25 are unsupported. TypeScript remains at 5.9.3 to satisfy the
   API generator and lint tooling peer contracts.
6. Upgrade policy workflows to Bundle Action v3, preferably pinning the reviewed
   release commit `129eb4612dff4903e33cddb3cc9e0db131c3dfb6`. It downloads Bundle
   CLI 0.3.0 and verifies its checksum. Existing v2/v2.0.0 tags retain their
   original CLI 0.1.0 default.

When upgrading from Core 0.1, also handle array-valued `attr` in Cedar JSON for
nested `has` expressions. Invalid action applications remain validation errors.
For label syntax, required response metadata, and endpoint migrations, see the
[original coordinated migration](MIGRATION.md).

## Release verification and immutable pins

Core 0.3.0 was released from `ac5a2a60c726ff399a18c6072f9641851be23145`;
Bundle 0.3.0 was released from `d46c2502e2e71b91b4eaa23bef43f36b27e8800b`.
REST 0.2.0 is signed at commit `33e46cd397f460c4c91a6e34fffc793e89c2e308`.
Consumer integration workflows use this published container:

```text
ghcr.io/treetop-policy-engine/treetop-rest@sha256:e29baeb5b498f21c72dccd9d08ab43198fa4705cecb40643d0ee97b63cc1c932
```

The frontend generates its API declarations and runs browser/demo checks against
that same REST source commit. Core and Bundle crates are registry dependencies
with committed lockfile checksums. Every release tag is signed and immutable.

| Repository | Release review |
| --- | --- |
| treetop-core | [#73](https://github.com/treetop-policy-engine/treetop-core/pull/73) |
| treetop-bundle | [#13](https://github.com/treetop-policy-engine/treetop-bundle/pull/13) |
| treetop-rest | [Core update #89](https://github.com/treetop-policy-engine/treetop-rest/pull/89), [dependencies #90](https://github.com/treetop-policy-engine/treetop-rest/pull/90), [release #91](https://github.com/treetop-policy-engine/treetop-rest/pull/91) |
| treetop-client | [Dependencies #17](https://github.com/treetop-policy-engine/treetop-client/pull/17), [release #18](https://github.com/treetop-policy-engine/treetop-client/pull/18) |
| treetop-client-python | [#18](https://github.com/treetop-policy-engine/treetop-client-python/pull/18) |
| treetop-client-go | [#6](https://github.com/treetop-policy-engine/treetop-client-go/pull/6) |
| treetop-cli | [Dependencies #11](https://github.com/treetop-policy-engine/treetop-cli/pull/11), [release #12](https://github.com/treetop-policy-engine/treetop-cli/pull/12) |
| treetop-frontend | [#7](https://github.com/treetop-policy-engine/treetop-frontend/pull/7) |
| treetop-bundle-action | [#11](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/11) |

REST retains Gungraun 0.19.4 for reliable instruction counts: 0.20 produced
repeatable zero-count schema measurements on the CI Valgrind version and changed
the disabled-access wrapper count. All 37 final benchmark comparisons have
positive measurements and remain within the existing 8% gate. Runtime dependency
updates are included. CLI uses Gungraun 0.20 with benchmark symbols preserved;
all three matrix benchmarks produce positive counts.

## September 2026 dependency refresh history

This refresh covers all ten repositories in `treetop-policy-engine`. Package
registries, upstream releases, and action commits were checked on 2026-09-19.
The implementation updates were squash-merged in dependency order on 2026-09-19.
At that point, Core and Bundle 0.2.0 were published; the other updates were on
`main` and awaited the versioned releases recorded above.

### September repository status

| Repository | Review | Scope and publication status |
| --- | --- | --- |
| Core | [#66](https://github.com/treetop-policy-engine/treetop-core/pull/66) | Merged; [Core 0.2.0 published](https://crates.io/crates/treetop-core/0.2.0), with Cedar 4.13.0 and refreshed dependencies/actions |
| Bundle | [#12](https://github.com/treetop-policy-engine/treetop-bundle/pull/12) | Merged; [Bundle 0.2.0 and native CLI published](https://github.com/treetop-policy-engine/treetop-bundle/releases/tag/v0.2.0) |
| REST | [#82](https://github.com/treetop-policy-engine/treetop-rest/pull/82) | Merged; published Core/Bundle 0.2.0 and Cedar 4.13.0; application/fuzz lockfiles and action updates |
| Rust client | [#16](https://github.com/treetop-policy-engine/treetop-client/pull/16) | Merged; dependencies, fuzz lockfile, actions, and Rust 1.93.1 minimum |
| Python client | [#16](https://github.com/treetop-policy-engine/treetop-client-python/pull/16) | Merged; build/type-check tools, lockfile, and actions |
| Go client | [#5](https://github.com/treetop-policy-engine/treetop-client-go/pull/5) | Merged; security and Markdown tools; no external module dependencies; actions already current |
| CLI | [#10](https://github.com/treetop-policy-engine/treetop-cli/pull/10) | Merged; dependencies, lockfile, actions, and pinned Rust builder image |
| Frontend | [#5](https://github.com/treetop-policy-engine/treetop-frontend/pull/5) | Merged; npm dependencies, lockfile, supported Node versions, and actions |
| Bundle Action | [#9](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/9) | Merged; Bundle CLI 0.2.0 default, immutable release source, action pins, and artifact upload example |
| Organization | [#4](https://github.com/treetop-policy-engine/.github/pull/4) | Documentation and action dependencies checked; existing pins already current |

These September implementation merges did not themselves create versioned
releases. The October releases above supersede their publication status. The
[original coordinated release](MIGRATION.md) records the earlier API migrations.

### September Core, Bundle, and REST rollout

1. Completed: Core 0.2.0 is published from verified main commit
   `6ebef45b46b1b725e48ab5704a64cf266da25860`.
2. Completed: Bundle 0.2.0 uses the published Core crate and is released from
   verified main commit `20c4661296cc23381bbce1d86a94949311823a11`. All four
   native CLI archives and `SHA256SUMS` are published.
3. Completed: REST's merged update resolves both published crates, with registry
   checksums in its application and fuzz lockfiles. Server release preparation
   remained a separate step at that time.
4. Completed: Bundle Action's merged update defaults to published CLI 0.2.0 and
   verifies its exact release source. That archive-compatibility change required
   the subsequent major Action release. Existing v2/v2.0.0 contracts remain fixed; users can
   explicitly select `binary-version: 0.2.0` with the published action.

Rebuild and re-sign policy archives with Bundle CLI 0.2.0 before deploying the
updated REST server. Archive generator metadata must match Bundle/Core 0.2.0
and Cedar 4.13.0 exactly; editing a signed archive is not a migration. Manifest
and signature format 2 and declared label targets remain unchanged. Cedar JSON
consumers must support array-valued `attr` for nested `has` expressions.

Cedar 4.13 classifies invalid action applications as warnings. Core and Bundle
preserve Treetop's strict validation contract by rejecting these diagnostics as
errors. Other Bundle warnings retain their existing `deny_warnings` behavior.

### September merge order

Core and Bundle were merged and released first. The remaining implementation PRs
were then squash-merged as REST, Rust/Python/Go clients, CLI, frontend, and Bundle
Action, followed by this organization record. Each merge used the verified PR
head and retained substantive rationale and migration notes in its squash body.
Published REST/client integration pins remain intentional prerequisites for
consumer checks; merging a newer implementation does not move those releases.

### Toolchain and compatibility constraints

- The September Rust client update raised the minimum to Rust 1.93.1 because
  its test and transitive dependencies could no longer support Rust 1.85. The
  then-published CLI retained Rust 1.89 against client 0.1.0; CLI 0.2.0 above
  now requires Rust 1.93.1.
- The frontend requires Node `^22.22.2 || ^24.15.0 || >=26` for its updated tools.
  TypeScript stays at 5.9.3: current `openapi-typescript` 7.13.0 requires TypeScript
  5.x, and `typescript-eslint` 8.70.0 excludes TypeScript 6.1 and later. Installing
  TypeScript 7.0.2 would violate those peer contracts. Revisit this holdback when
  both tools support it.
- Third-party actions and reusable workflows use immutable reviewed commits.
  Rust toolchain actions tracking upstream `master` explicitly select `stable`.
- Existing released REST images/source and client versions remain valid pinned
  integration prerequisites. An unpublished branch is not a new release.

Each implementation PR records its own verification. Dependency updates retain
formatting, lint, test, documentation, audit, package, and performance gates;
local environmental limitations are documented rather than treated as passes.
