# September 2026 dependency refresh

This refresh covers all ten repositories in `treetop-policy-engine`. Package
registries, upstream releases, and action commits were checked on 2026-09-19.
Updates remain in the linked pull requests unless marked as released below.

## Repository status

| Repository | Review | Scope and publication status |
| --- | --- | --- |
| Core | [#66](https://github.com/treetop-policy-engine/treetop-core/pull/66) | Merged; [Core 0.2.0 published](https://crates.io/crates/treetop-core/0.2.0), with Cedar 4.13.0 and refreshed dependencies/actions |
| Bundle | [#12](https://github.com/treetop-policy-engine/treetop-bundle/pull/12) | Merged; [Bundle 0.2.0 and native CLI published](https://github.com/treetop-policy-engine/treetop-bundle/releases/tag/v0.2.0) |
| REST | [#82](https://github.com/treetop-policy-engine/treetop-rest/pull/82) | Open; published Core/Bundle 0.2.0 and Cedar 4.13.0; application/fuzz lockfiles and action updates |
| Rust client | [#16](https://github.com/treetop-policy-engine/treetop-client/pull/16) | Open; dependencies, fuzz lockfile, actions, and Rust 1.93.1 minimum |
| Python client | [#16](https://github.com/treetop-policy-engine/treetop-client-python/pull/16) | Open; build/type-check tools, lockfile, and actions |
| Go client | [#5](https://github.com/treetop-policy-engine/treetop-client-go/pull/5) | Open; security and Markdown tools; no external module dependencies; actions already current |
| CLI | [#10](https://github.com/treetop-policy-engine/treetop-cli/pull/10) | Open; dependencies, lockfile, actions, and pinned Rust builder image |
| Frontend | [#5](https://github.com/treetop-policy-engine/treetop-frontend/pull/5) | Open; npm dependencies, lockfile, supported Node versions, and actions |
| Bundle Action | [#9](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/9) | Open; Bundle CLI 0.2.0 default, immutable release source, action pins, and artifact upload example |
| Organization | [#4](https://github.com/treetop-policy-engine/.github/pull/4) | Documentation and action dependencies checked; existing pins already current |

No release is implied by an open dependency PR. REST, clients, CLI, frontend,
and Bundle Action retain their existing published releases until separately
reviewed and released. The [previous coordinated release](MIGRATION.md) remains
the migration reference for those artifacts.

## Core, Bundle, and REST rollout

1. Completed: Core 0.2.0 is published from verified main commit
   `6ebef45b46b1b725e48ab5704a64cf266da25860`.
2. Completed: Bundle 0.2.0 uses the published Core crate and is released from
   verified main commit `20c4661296cc23381bbce1d86a94949311823a11`. All four
   native CLI archives and `SHA256SUMS` are published.
3. REST's open PR resolves both published crates, with registry checksums in its
   application and fuzz lockfiles. Review and release REST separately.
4. Bundle Action's open PR defaults to published CLI 0.2.0 and verifies its exact
   release source. This archive-compatibility change requires a new major Action
   release after review. Existing v2/v2.0.0 contracts remain fixed; users can
   explicitly select `binary-version: 0.2.0` with the published action.

Rebuild and re-sign policy archives with Bundle CLI 0.2.0 before deploying the
updated REST server. Archive generator metadata must match Bundle/Core 0.2.0
and Cedar 4.13.0 exactly; editing a signed archive is not a migration. Manifest
and signature format 2 and declared label targets remain unchanged. Cedar JSON
consumers must support array-valued `attr` for nested `has` expressions.

Cedar 4.13 classifies invalid action applications as warnings. Core and Bundle
preserve Treetop's strict validation contract by rejecting these diagnostics as
errors. Other Bundle warnings retain their existing `deny_warnings` behavior.

## Toolchain and compatibility constraints

- The Rust client now requires Rust 1.93.1. Its current test and transitive
  dependency graph cannot support the former Rust 1.85 minimum. CLI retains its
  separately verified Rust 1.89 minimum against the published client 0.1.0.
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
