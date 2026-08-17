<p align="center">
  <img src="./profile/assets/treetop-lockup.svg" alt="Treetop" width="420">
</p>

# Treetop Policy Engine

Treetop is an open-source authorization stack built on
[Cedar](https://cedarpolicy.com/). It separates access-control policy from application code and
provides a consistent path from authoring and validation to deployment, evaluation, and operations.

Most applications use Treetop through the standalone REST service. Official Rust and Python
clients, a command-line client, and a browser workbench cover integration and day-to-day operation.
The Rust engine powers the service and remains available for specialized deployments that require
in-process evaluation.

## Goals

- Make fine-grained authorization explicit, testable, and independent of application business
  logic.
- Make centralized authorization straightforward to deploy and consume through a stable REST API
  and typed clients.
- Retain an embeddable Rust engine for specialized applications that need in-process evaluation.
- Package independently owned policy modules into deterministic, verifiable release artifacts.
- Make policy systems practical to operate with typed clients, validation tooling, metrics, and
  human-facing inspection tools.
- Keep the complete stack open source and usable through versioned libraries, binaries, containers,
  and automation.

## Functionality

Treetop REST evaluates Cedar requests expressed as a principal, action, resource, and optional
context. The underlying policy engine supports schemas, group membership, and resource attributes
derived through label rules.

The wider toolchain provides:

- validation and deterministic composition of policies, schemas, labels, and module manifests;
- optional Ed25519 signing and verification of policy bundles;
- atomic bundle loading in the REST server;
- authorization, policy, schema, bundle, OpenAPI, and metrics endpoints;
- typed Rust and Python integrations;
- native command-line tools for authorization and bundle workflows;
- a browser workbench for requests, policies, schemas, server status, and metrics; and
- a GitHub Action for policy validation and unsigned bundle builds in CI.

The typical delivery path is:

```text
Cedar policies, schemas, and labels
              ↓
    validation and bundling
              ↓
       Treetop REST service
              ↓
 applications, CLI, and workbench
```

Specialized Rust applications can use `treetop-core` directly when a separate service is not the
right deployment model.

## Projects

All repositories below are maintained parts of the Treetop project. Together they provide the REST
runtime, policy engine and delivery pipeline, client integrations, and operator tooling.

| Project | Role |
| --- | --- |
| [treetop-rest](https://github.com/treetop-policy-engine/treetop-rest) | Standalone authorization server and REST API |
| [treetop-client](https://github.com/treetop-policy-engine/treetop-client) | Typed asynchronous Rust client |
| [treetop-client-python](https://github.com/treetop-policy-engine/treetop-client-python) | Typed synchronous and asynchronous Python client |
| [treetop-cli](https://github.com/treetop-policy-engine/treetop-cli) | Command-line client and interactive REPL |
| [treetop-frontend](https://github.com/treetop-policy-engine/treetop-frontend) | Browser workbench for Treetop servers |
| [treetop-bundle](https://github.com/treetop-policy-engine/treetop-bundle) | Policy validation, composition, signing, and archive tooling |
| [treetop-bundle-action](https://github.com/treetop-policy-engine/treetop-bundle-action) | Bundle validation and builds in GitHub Actions |
| [treetop-core](https://github.com/treetop-policy-engine/treetop-core) | Rust policy engine underlying Treetop REST, also available for in-process deployments |

See the [organization profile](./profile/README.md) for installation and release artifacts.

## About this repository

This repository publishes organization-wide GitHub content:

- [`profile/README.md`](./profile/README.md) is rendered on the organization's public profile.
- [`profile/assets`](./profile/assets) contains the Treetop lockup, mark, and avatar variants.

When updating the profile, keep the project catalog aligned with the organization's public
repositories and published artifacts. Prefer durable links such as `/releases/latest`, registry
project pages, GHCR package pages, and moving major tags for GitHub Actions. Use relative asset paths
and verify Markdown links before publishing changes.
