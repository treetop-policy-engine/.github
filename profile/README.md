<p align="center">
  <img src="./assets/treetop-lockup.svg" alt="Treetop" width="420">
</p>

# Treetop Policy Engine

Treetop is an open-source authorization stack built on [Cedar](https://cedarpolicy.com/). It
provides tools for building, validating, serving, and operating policy-based authorization.

Run Treetop as a REST service, integrate through an official client, compose deterministic policy
bundles, or inspect the service from the command line and browser workbench. The Rust engine can
also be embedded for specialized use cases.

## Coordinated breaking release

The coordinated release adopts declared label ownership by exact Cedar resource type
and attribute. Early Treetop releases prioritize correctness over compatibility;
ambiguous ownership, deprecated aliases, and old-format defaults are removed.
Bundle/module format 2 requires rebuilt and re-signed archives.

The September 6 release shipped Core, Bundle, REST, Rust/Python clients, CLI, and Workbench as 0.1.0;
the Go client shipped as 0.3.0 and Bundle Action as v2. See the
[breaking migration and release set](../MIGRATION.md) for the new syntax, required consumer
updates, published artifacts, and upgrade order.

## Current coordinated releases

Core and Bundle 0.4.0 use Cedar 4.15.0. Before deploying REST 0.3.0, rebuild
and re-sign policy archives with Bundle CLI 0.4.0 or Bundle Action v4.

Rust SDK 0.2.1, Python SDK 0.1.2, Go SDK 0.4.0 and Workbench 0.2.1 target
REST 0.3.0. CLI 0.3.0 uses Rust SDK 0.2.1 and requires Rust 1.95.0 for source
builds. [Release downloads, immutable pins, and migration notes](../DEPENDENCIES.md).

## Projects

These repositories are all maintained parts of Treetop, covering the runtime, policy delivery,
client integrations, and operator tooling.

| Project | Purpose | Distribution |
| --- | --- | --- |
| [treetop-rest](https://github.com/treetop-policy-engine/treetop-rest) | REST API and standalone server | [server archives](https://github.com/treetop-policy-engine/treetop-rest/releases/latest) · [container](https://github.com/treetop-policy-engine/treetop-rest/pkgs/container/treetop-rest) |
| [treetop-client](https://github.com/treetop-policy-engine/treetop-client) | Typed asynchronous Rust client | [crate](https://crates.io/crates/treetop-client) · [docs](https://docs.rs/treetop-client) |
| [treetop-client-python](https://github.com/treetop-policy-engine/treetop-client-python) | Typed synchronous and asynchronous Python client | [PyPI](https://pypi.org/project/treetop-client/) |
| [treetop-client-go](https://github.com/treetop-policy-engine/treetop-client-go) | Typed Go client | [Go package](https://pkg.go.dev/github.com/treetop-policy-engine/treetop-client-go) |
| [treetop-cli](https://github.com/treetop-policy-engine/treetop-cli) | Command-line client and interactive REPL | [native archives and checksums](https://github.com/treetop-policy-engine/treetop-cli/releases/latest) |
| [treetop-frontend](https://github.com/treetop-policy-engine/treetop-frontend) | Browser workbench for policies, requests, and metrics | [static archive and checksum](https://github.com/treetop-policy-engine/treetop-frontend/releases/latest) · [container](https://github.com/treetop-policy-engine/treetop-frontend/pkgs/container/treetop-frontend) |
| [treetop-bundle](https://github.com/treetop-policy-engine/treetop-bundle) | Deterministic, optionally signed Cedar policy bundles | [crate](https://crates.io/crates/treetop-bundle) · [docs](https://docs.rs/treetop-bundle) · [CLI archives](https://github.com/treetop-policy-engine/treetop-bundle/releases/latest) |
| [treetop-bundle-action](https://github.com/treetop-policy-engine/treetop-bundle-action) | Policy validation and bundle builds in GitHub Actions | [v4.0.0](https://github.com/treetop-policy-engine/treetop-bundle-action/releases/tag/v4.0.0) · [releases](https://github.com/treetop-policy-engine/treetop-bundle-action/releases) |
| [treetop-core](https://github.com/treetop-policy-engine/treetop-core) | Rust engine underlying Treetop REST, also available for in-process deployments | [crate](https://crates.io/crates/treetop-core) · [docs](https://docs.rs/treetop-core) |

## Release artifacts

| Artifact | Published formats |
| --- | --- |
| Treetop server | Linux x86-64 and ARM64 musl archives; container image |
| Treetop CLI | Linux x86-64 and ARM64 musl, Apple-silicon macOS, and Windows x86-64 archives with SHA-256 checksums |
| Bundle CLI | Linux x86-64 and ARM64 musl, Apple-silicon macOS, and Windows x86-64 archives with `SHA256SUMS` |
| Workbench | Versioned static-site archive with a SHA-256 checksum; Linux AMD64 and ARM64 container image |
| Libraries and clients | Rust crates, Python on PyPI, and versioned Go modules |
| Bundle Action | Versioned GitHub Action v4.0.0 with the v4 major tag |

## Quick start

Run the server:

```bash
docker run --rm --publish 9999:9999 \
  --env TREETOP_LISTEN=0.0.0.0 \
  ghcr.io/treetop-policy-engine/treetop-rest:latest
```

Then check process liveness:

```bash
curl http://127.0.0.1:9999/livez
```

Bundle Action v4 downloads and verifies Bundle CLI 0.4.0. Pin the release
commit `145b69752ed7daaf4fe1071f0fc6e5c6e2899e0d` in protected workflows.
[Bundle Action v4 migration](https://github.com/treetop-policy-engine/treetop-bundle-action/blob/v4.0.0/MIGRATION.md).

Project-specific documentation, examples, current contracts, and release notes live in
each repository.
