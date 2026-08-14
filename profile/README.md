<p align="center">
  <img src="./assets/treetop-lockup.svg" alt="Treetop" width="420">
</p>

# Treetop Policy Engine

Treetop is an open-source, Cedar-based authorization stack for embedding, serving, and operating
policy decisions.

Use the engine directly from Rust, run it as a REST service, integrate through an official client,
or inspect it from the command line and browser workbench.

## Projects

| Project | Purpose | Distribution |
| --- | --- | --- |
| [treetop-core](https://github.com/treetop-policy-engine/treetop-core) | Embeddable Rust policy engine | [crates.io](https://crates.io/crates/treetop-core) |
| [treetop-rest](https://github.com/treetop-policy-engine/treetop-rest) | REST API and standalone server | [Releases](https://github.com/treetop-policy-engine/treetop-rest/releases) · [GHCR](https://github.com/treetop-policy-engine/treetop-rest/pkgs/container/treetop-rest) |
| [treetop-client](https://github.com/treetop-policy-engine/treetop-client) | Typed asynchronous Rust client | [crates.io](https://crates.io/crates/treetop-client) |
| [treetop-client-python](https://github.com/treetop-policy-engine/treetop-client-python) | Typed synchronous and asynchronous Python client | [PyPI](https://pypi.org/project/treetop-client/) |
| [treetop-cli](https://github.com/treetop-policy-engine/treetop-cli) | Command-line client and interactive REPL | [Releases](https://github.com/treetop-policy-engine/treetop-cli/releases) |
| [treetop-frontend](https://github.com/treetop-policy-engine/treetop-frontend) | Browser workbench for policies, requests, and metrics | [Releases](https://github.com/treetop-policy-engine/treetop-frontend/releases) |

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

Project-specific documentation, examples, and release notes live in each repository.
