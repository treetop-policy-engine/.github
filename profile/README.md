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

Core, Bundle, REST, Rust/Python clients, CLI, and Workbench are released as 0.1.0;
the Go client is 0.3.0 and Bundle Action is v2. See the
[breaking migration and release set](../MIGRATION.md) for the new syntax, required consumer
updates, published artifacts, and upgrade order.

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
| [treetop-bundle-action](https://github.com/treetop-policy-engine/treetop-bundle-action) | Policy validation and bundle builds in GitHub Actions | [v2.0.0](https://github.com/treetop-policy-engine/treetop-bundle-action/releases/tag/v2.0.0) · [releases](https://github.com/treetop-policy-engine/treetop-bundle-action/releases) |
| [treetop-core](https://github.com/treetop-policy-engine/treetop-core) | Rust engine underlying Treetop REST, also available for in-process deployments | [crate](https://crates.io/crates/treetop-core) · [docs](https://docs.rs/treetop-core) |

## Release artifacts

| Artifact | Published formats |
| --- | --- |
| Treetop server | Linux x86-64 and ARM64 musl archives; container image |
| Treetop CLI | Linux x86-64 and ARM64 musl, Apple-silicon macOS, and Windows x86-64 archives with SHA-256 checksums |
| Bundle CLI | Linux x86-64 and ARM64 musl, Apple-silicon macOS, and Windows x86-64 archives with `SHA256SUMS` |
| Workbench | Versioned static-site archive with a SHA-256 checksum; Linux AMD64 and ARM64 container image |
| Libraries and clients | Rust crates, Python on PyPI, and versioned Go modules |
| Bundle Action | Versioned GitHub Action; v2.0.0 with the v2 major tag |

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

The [action v2 migration](https://github.com/treetop-policy-engine/treetop-bundle-action/blob/v2.0.0/MIGRATION.md)
explains declared-target syntax and format 2 archives. Action v2 downloads the
published Bundle CLI 0.1.0 and verifies its checksum. Pin the reviewed immutable
action release commit in protected policy workflows.

Project-specific documentation, examples, current contracts, and release notes live in
each repository.
