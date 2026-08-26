# Durable Execution SDK Workspace

This repository provides a single workspace for active public SDKs that
implement the AWS Lambda Durable Execution programming model, together with the
shared conformance, CI, and documentation repositories.

Each component remains independently versioned and is included as a Git
submodule pinned to a specific commit. Inclusion records availability and does
not imply AWS endorsement or production support.

## AWS-maintained SDKs

| Repository (path) | Language | Checks |
| --- | --- | --- |
| [`aws/aws-durable-execution-sdk-java`](https://github.com/aws/aws-durable-execution-sdk-java) (`aws-maintained/java`) | Java 17+ | [![Build](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/build.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/build.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/conformance-tests.yml) [![OpenTelemetry Conformance](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/otel-conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/otel-conformance-tests.yml) |
| [`aws/aws-durable-execution-sdk-js`](https://github.com/aws/aws-durable-execution-sdk-js) (`aws-maintained/js`) | JavaScript and TypeScript (Node.js 22.x, 24.x) | [![Build](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/build.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/build.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/conformance-tests.yml) [![OpenTelemetry Conformance](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/otel-conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/otel-conformance-tests.yml) |
| [`aws/aws-durable-execution-sdk-python`](https://github.com/aws/aws-durable-execution-sdk-python) (`aws-maintained/python`) | Python 3.11-3.14 | [![Build](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/ci.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/ci.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/conformance-tests.yml) [![OpenTelemetry Conformance](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/opentelemetry-conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/opentelemetry-conformance-tests.yml) |
| [`aws/aws-durable-execution-sdk-rust`](https://github.com/aws/aws-durable-execution-sdk-rust) (`aws-maintained/rust`) | Rust 1.94.1+ (experimental preview) | [![CI](https://github.com/aws/aws-durable-execution-sdk-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-rust/actions/workflows/ci.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-rust/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-rust/actions/workflows/conformance-tests.yml) |
| [`aws/aws-lambda-dotnet`](https://github.com/aws/aws-lambda-dotnet) (`aws-maintained/dotnet`)<sup>1</sup> | .NET 8.0, 10.0 | [![Build](https://github.com/aws/aws-lambda-dotnet/actions/workflows/aws-ci.yml/badge.svg)](https://github.com/aws/aws-lambda-dotnet/actions/workflows/aws-ci.yml) [![Conformance](https://github.com/aws/aws-lambda-dotnet/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-lambda-dotnet/actions/workflows/conformance-tests.yml) |

<sup>1</sup> The .NET SDK is under
`Libraries/src/Amazon.Lambda.DurableExecution`; this submodule contains the full
AWS Lambda .NET monorepo.

## Supporting repositories

| Repository (path) | Checks | Purpose |
| --- | --- | --- |
| [`aws/aws-durable-execution-conformance-tests`](https://github.com/aws/aws-durable-execution-conformance-tests) (`supporting/conformance-tests`) | [![Build](https://github.com/aws/aws-durable-execution-conformance-tests/actions/workflows/ci.yml/badge.svg)](https://github.com/aws/aws-durable-execution-conformance-tests/actions/workflows/ci.yml) | Language-neutral requirements, runner, and reusable conformance workflows. |
| [`aws/aws-durable-execution-ci`](https://github.com/aws/aws-durable-execution-ci) (`supporting/ci`) | [![AI PR Review](https://github.com/aws/aws-durable-execution-ci/actions/workflows/ai-pr-review.yml/badge.svg)](https://github.com/aws/aws-durable-execution-ci/actions/workflows/ai-pr-review.yml) | Shared GitHub Actions automation. |
| [`aws/aws-durable-execution-docs`](https://github.com/aws/aws-durable-execution-docs) (`supporting/docs`) | [![Docs](https://github.com/aws/aws-durable-execution-docs/actions/workflows/docs.yml/badge.svg)](https://github.com/aws/aws-durable-execution-docs/actions/workflows/docs.yml) [![Quality](https://github.com/aws/aws-durable-execution-docs/actions/workflows/quality.yml/badge.svg)](https://github.com/aws/aws-durable-execution-docs/actions/workflows/quality.yml) | Cross-language AWS Lambda Durable Execution documentation. |

## Community SDKs

| Repository (path) | Language | Status | Checks | Validation and notes |
| --- | --- | --- | --- | --- |
| [`zhongkechen/async-durable-execution`](https://github.com/zhongkechen/async-durable-execution) (`community/python-async`) | Python 3.10+ | Conformance-tested | [![Build](https://github.com/zhongkechen/async-durable-execution/actions/workflows/build.yml/badge.svg)](https://github.com/zhongkechen/async-durable-execution/actions/workflows/build.yml) [![Conformance](https://github.com/zhongkechen/async-durable-execution/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/zhongkechen/async-durable-execution/actions/workflows/conformance-tests.yml) | Async-first fork that runs every upstream conformance suite with failed and uncovered requirements treated as failures. |
| [`zhongkechen/durable-execution-cpp`](https://github.com/zhongkechen/durable-execution-cpp) (`community/cpp`) | C++23 | Conformance-tested | [![CI](https://github.com/zhongkechen/durable-execution-cpp/actions/workflows/ci.yml/badge.svg)](https://github.com/zhongkechen/durable-execution-cpp/actions/workflows/ci.yml) | Runs the full upstream conformance suite in repository CI with failed and uncovered requirements treated as failures. |
| [`kurochan/aws-durable-execution-go`](https://github.com/kurochan/aws-durable-execution-go) (`community/go-kurochan`) | Go 1.25+ | Experimental | [![Tests](https://github.com/kurochan/aws-durable-execution-go/actions/workflows/test.yml/badge.svg)](https://github.com/kurochan/aws-durable-execution-go/actions/workflows/test.yml) | Self-described unofficial and experimental implementation. Repository CI runs its Go tests; stricter SDK compatibility coverage remains planned. |
| [`dgr237/aws-durable-execution-sdk-go`](https://github.com/dgr237/aws-durable-execution-sdk-go) (`community/go-dgr237`) | Go 1.24+ | Self-assessed | No repository CI | Provides unit tests, local replay testing utilities, static analysis, and a specification compliance report. It does not run compatibility tests in repository CI or the upstream conformance suite. |
| [`pgdad/durable-rust`](https://github.com/pgdad/durable-rust) (`community/rust-pgdad`) | Rust 1.82+ | Independently tested | [![CI](https://github.com/pgdad/durable-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/pgdad/durable-rust/actions/workflows/ci.yml) | Provides a Python-Rust compliance suite and internal parity tests, but does not run the upstream conformance suite. |
| [`alessandrobologna/lambda-durable-execution-rust`](https://github.com/alessandrobologna/lambda-durable-execution-rust) (`community/rust-alessandrobologna`) | Rust 1.88+ | Experimental | [![CI](https://github.com/alessandrobologna/lambda-durable-execution-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrobologna/lambda-durable-execution-rust/actions/workflows/ci.yml) | Self-described experimental implementation exercised primarily in the maintainer's workloads; repository CI runs Rust tests and lint checks. |
| [`bnusunny/durable-execution-rust-sdk`](https://github.com/bnusunny/durable-execution-rust-sdk) (`community/rust-bnusunny`) | Rust 1.91.1+ | Independently tested | [![PR validation](https://github.com/bnusunny/durable-execution-rust-sdk/actions/workflows/pr.yml/badge.svg)](https://github.com/bnusunny/durable-execution-rust-sdk/actions/workflows/pr.yml) | Runs lint, unit, replay, history, and cross-SDK format tests in CI and provides a manually dispatched cloud integration workflow. It does not run the upstream conformance suite. |

### Status definitions

- **Conformance-tested:** A community SDK runs the AWS Durable Execution
  conformance suite in CI.
- **Independently tested:** A community SDK has its own compatibility or parity
  tests but does not run the upstream conformance suite.
- **Self-assessed:** A community SDK documents its compatibility and has local
  tests but does not run compatibility tests in repository CI.
- **Experimental:** The repository describes itself as experimental and does
  not run the upstream conformance suite.

Community statuses describe the submodule revision recorded by this repository
and may change as the component projects evolve.

The Go and Rust repositories are independent implementations. Their inclusion
does not designate any one as the canonical community SDK for its language.

## Scope

A repository is included when it is public and active and either:

- implements the high-level Lambda Durable Execution handler and durable
  operation programming model; or
- provides shared conformance, CI, or documentation for those SDKs.

Generated low-level AWS Lambda service clients, application demos, deployment
frameworks, emulators, archived repositories, and empty proof-of-concept
repositories are outside this workspace's scope.

## Working with the workspace

### Clone

Initialize every repository:

```bash
git clone --recurse-submodules https://github.com/zhongkechen/durable-execution-sdk.git
```

To exclude community SDKs, clone the workspace without submodules and
initialize only the AWS-maintained SDKs and supporting repositories:

```bash
git clone https://github.com/zhongkechen/durable-execution-sdk.git
cd durable-execution-sdk
git submodule update --init --recursive -- aws-maintained supporting
```

The repositories under `community/` remain uninitialized. Initialize an
individual community SDK later by its path:

```bash
git submodule update --init --recursive -- community/go-kurochan
```

To initialize every submodule in an existing clone:

```bash
git submodule update --init --recursive
```

### Update

Pull the workspace and check out its recorded component revisions:

```bash
git pull --recurse-submodules
git submodule update --init --recursive
```

Changes must be committed in the component repository first. Commit the updated
submodule pointer in this repository after the component commit is available
from its remote.

### Cross-repository contributions

Each submodule is an independent repository with its own contribution process,
branches, commits, and pull requests. Make source changes in the repository
that owns the behavior:

- SDK implementation, language-specific tests, examples, and conformance
  handlers belong in the affected SDK repository.
- Language-neutral requirements, the conformance runner, and reusable
  conformance workflows belong in `supporting/conformance-tests`.
- Shared GitHub Actions automation belongs in `supporting/ci`.
- Cross-language documentation belongs in `supporting/docs`.

For a change that spans repositories:

1. Create a branch inside each affected submodule. Submodules are checked out
   at detached commits by default, so switch to a branch before committing.
2. Develop and commit each component change in its owning repository.
3. Check out the related branches or commits together in this workspace and
   run the affected SDK and conformance tests.
4. Open separate component pull requests and link the related pull requests in
   their descriptions. Identify any required merge order.
5. After the component commits are available from their remotes, open a
   workspace pull request that updates the corresponding submodule pointers.

Committing at the workspace root records only submodule commit pointers; it
does not commit uncommitted files inside a submodule. Use
`git submodule foreach git status --short --branch` to review component state
before updating the workspace pointers.
