# Core 0.3.0 rollout

Core 0.3.0 was released on 2026-10-02. Bundle 0.3.0 and all four native CLI
archives were published on 2026-10-03. The coordinated downstream updates use
published crate dependencies and immutable source pins. Other artifact versions
remain unchanged until their separate release preparation and publication.

## Current repository changes

| Repository | Review | Core 0.3.0 scope |
| --- | --- | --- |
| Core | [#73](https://github.com/treetop-policy-engine/treetop-core/pull/73) | [0.3.0 published](https://crates.io/crates/treetop-core/0.3.0); Utoipa 6 and compact immutable permit JSON |
| Bundle | [#13](https://github.com/treetop-policy-engine/treetop-bundle/pull/13) | [0.3.0 library and native CLI published](https://github.com/treetop-policy-engine/treetop-bundle/releases/tag/v0.3.0); exact Core 0.3.0 dependency and generator metadata |
| REST | [#89](https://github.com/treetop-policy-engine/treetop-rest/pull/89) | Exact published Core/Bundle 0.3.0 dependencies in application and fuzz lockfiles; Utoipa 6, Swagger UI 10, and regenerated OpenAPI |
| Bundle Action | [#10](https://github.com/treetop-policy-engine/treetop-bundle-action/pull/10) | Main defaults to published CLI 0.3.0; native source and real release-download checks on all four platforms |
| Frontend | [#6](https://github.com/treetop-policy-engine/treetop-frontend/pull/6) | API generation, live tests, demos, and Docker context pin REST revision `a39da3952bf32ecbaab84dea4fe3e4471ca033fb` with published dependencies |
| Rust client | Existing strict HTTP contract | No Core dependency or wire-model change required |
| Python client | Existing strict HTTP contract | No Core dependency or wire-model change required |
| Go client | Existing strict HTTP contract | No Core dependency or wire-model change required |
| CLI | Existing client contract | No Core dependency or wire-model change required |
| Organization | This rollout record | Current dependency status, migration guidance, and project profile |

Core was released from verified main commit
`ac5a2a60c726ff399a18c6072f9641851be23145`. Bundle was released from verified main
commit `d46c2502e2e71b91b4eaa23bef43f36b27e8800b`; its signed `v0.3.0` tag
publishes the library, native CLI archives, and `SHA256SUMS`.

## Migration to Core and Bundle 0.3.0

1. Rust applications composing Core OpenAPI schemas must upgrade to Utoipa 6
   and compatible integrations. `PermitPolicy.json` now uses `Arc<PolicyJson>`;
   use `to_value()` to inspect or edit a JSON tree and `Arc::new(value.into())`
   in struct literals. `PermitPolicy::new` still accepts `serde_json::Value`.
2. Rebuild and re-sign policy archives with Bundle CLI 0.3.0 before deploying
   the updated REST server. The exact generator tuple is Bundle 0.3.0, Core
   0.3.0, and Cedar 4.13.0. Do not edit signed archives. Manifest and signature
   format 2 and declared label targets are unchanged.
3. Deploy REST from the reviewed dependency update. Both Cargo lockfiles use
   registry checksums; no temporary Bundle Git dependency remains. The server
   package version is still 0.1.0, so use the immutable source revision to select
   this update until a new server release exists.
4. Update the frontend's immutable REST source and regenerate API declarations.
   The reviewed schema changes only adjust nullable-reference ordering and
   permit-JSON documentation. HTTP request and decision JSON are unchanged;
   current strict-contract SDKs and CLI clients need no model changes.
5. For published Bundle Action v2.0.0, set `binary-version: 0.3.0` explicitly.
   Its existing CLI 0.1.0 default and v2 tags remain fixed. The main branch's new
   archive-compatibility default requires a future major Action release.

When upgrading from Core 0.1, also handle array-valued `attr` in Cedar JSON for
nested `has` expressions. Invalid action applications remain validation errors.
For label syntax, required response metadata, and endpoint migrations, see the
[original coordinated migration](MIGRATION.md).

## September 2026 dependency refresh history

This refresh covers all ten repositories in `treetop-policy-engine`. Package
registries, upstream releases, and action commits were checked on 2026-09-19.
The implementation updates were squash-merged in dependency order on 2026-09-19.
Core and Bundle 0.2.0 are published; the other updates are on `main` and await
separate versioned releases.

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

A merged dependency update does not create a versioned release. REST, clients,
CLI, frontend, and Bundle Action retain their existing published releases until
separate release preparation and publication. The [previous coordinated release](MIGRATION.md) remains
the migration reference for those artifacts.

### September Core, Bundle, and REST rollout

1. Completed: Core 0.2.0 is published from verified main commit
   `6ebef45b46b1b725e48ab5704a64cf266da25860`.
2. Completed: Bundle 0.2.0 uses the published Core crate and is released from
   verified main commit `20c4661296cc23381bbce1d86a94949311823a11`. All four
   native CLI archives and `SHA256SUMS` are published.
3. Completed: REST's merged update resolves both published crates, with registry
   checksums in its application and fuzz lockfiles. Prepare and release the next
   server version separately.
4. Completed: Bundle Action's merged update defaults to published CLI 0.2.0 and
   verifies its exact release source. This archive-compatibility change requires
   a new major Action release. Existing v2/v2.0.0 contracts remain fixed; users can
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
