# Durable Execution SDK Workspace

This repository provides a single workspace for active public SDKs that
implement the AWS Lambda Durable Execution programming model, together with the
shared conformance, CI, and documentation repositories.

Each component remains independently versioned and is included as a Git
submodule pinned to a specific commit. Inclusion records availability and does
not imply AWS endorsement or production support.

## Community status definitions

| Status | Meaning |
| --- | --- |
| Community, conformance-tested | A community SDK runs the AWS Durable Execution conformance suite in CI. |
| Community, independently tested | A community SDK has its own compatibility or parity tests but does not run the upstream conformance suite. |
| Community, experimental | The repository describes itself as experimental and does not run the upstream conformance suite. |

Community statuses describe the submodule revision recorded by this repository
and may change as the component projects evolve.

## AWS-maintained SDKs

| Path | Language | Repository | Checks | Validation and notes |
| --- | --- | --- | --- | --- |
| `aws-maintained/java` | Java | [`aws/aws-durable-execution-sdk-java`](https://github.com/aws/aws-durable-execution-sdk-java) | [![Build](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/build.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/build.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-java/actions/workflows/conformance-tests.yml) | Runs the upstream conformance suite in repository CI. |
| `aws-maintained/js` | JavaScript and TypeScript | [`aws/aws-durable-execution-sdk-js`](https://github.com/aws/aws-durable-execution-sdk-js) | [![Build](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/build.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/build.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-js/actions/workflows/conformance-tests.yml) | Runs the upstream conformance suite in repository CI. |
| `aws-maintained/python` | Python | [`aws/aws-durable-execution-sdk-python`](https://github.com/aws/aws-durable-execution-sdk-python) | [![Build](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/ci.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/ci.yml) [![Conformance](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-durable-execution-sdk-python/actions/workflows/conformance-tests.yml) | Runs the upstream conformance suite in repository CI. |
| `aws-maintained/dotnet` | .NET | [`aws/aws-lambda-dotnet`](https://github.com/aws/aws-lambda-dotnet) | [![Build](https://github.com/aws/aws-lambda-dotnet/actions/workflows/aws-ci.yml/badge.svg)](https://github.com/aws/aws-lambda-dotnet/actions/workflows/aws-ci.yml) [![Conformance](https://github.com/aws/aws-lambda-dotnet/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/aws/aws-lambda-dotnet/actions/workflows/conformance-tests.yml) | Runs the upstream conformance suite in repository CI. The SDK is under `Libraries/src/Amazon.Lambda.DurableExecution`; this submodule contains the full AWS Lambda .NET monorepo. |

## Community SDKs

| Path | Language | Repository | Status | Checks | Validation and notes |
| --- | --- | --- | --- | --- | --- |
| `community/python-async` | Python | [`zhongkechen/async-durable-execution`](https://github.com/zhongkechen/async-durable-execution) | Community, conformance-tested | [![Build](https://github.com/zhongkechen/async-durable-execution/actions/workflows/build.yml/badge.svg)](https://github.com/zhongkechen/async-durable-execution/actions/workflows/build.yml) [![Conformance](https://github.com/zhongkechen/async-durable-execution/actions/workflows/conformance-tests.yml/badge.svg)](https://github.com/zhongkechen/async-durable-execution/actions/workflows/conformance-tests.yml) | Async-first fork that runs every upstream conformance suite with failed and uncovered requirements treated as failures. |
| `community/go` | Go | [`kurochan/aws-durable-execution-go`](https://github.com/kurochan/aws-durable-execution-go) | Community, experimental | [![Tests](https://github.com/kurochan/aws-durable-execution-go/actions/workflows/test.yml/badge.svg)](https://github.com/kurochan/aws-durable-execution-go/actions/workflows/test.yml) | Self-described unofficial and experimental implementation. Repository CI runs its Go tests; stricter SDK compatibility coverage remains planned. |
| `community/rust-pgdad` | Rust | [`pgdad/durable-rust`](https://github.com/pgdad/durable-rust) | Community, independently tested | [![CI](https://github.com/pgdad/durable-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/pgdad/durable-rust/actions/workflows/ci.yml) | Provides a Python-Rust compliance suite and internal parity tests, but does not run the upstream conformance suite. |
| `community/rust-alessandrobologna` | Rust | [`alessandrobologna/lambda-durable-execution-rust`](https://github.com/alessandrobologna/lambda-durable-execution-rust) | Community, experimental | [![CI](https://github.com/alessandrobologna/lambda-durable-execution-rust/actions/workflows/ci.yml/badge.svg)](https://github.com/alessandrobologna/lambda-durable-execution-rust/actions/workflows/ci.yml) | Self-described experimental implementation exercised primarily in the maintainer's workloads; repository CI runs Rust tests and lint checks. |

The two Rust repositories are independent implementations. Their inclusion does
not designate either one as the canonical community Rust SDK.

## Supporting repositories

| Path | Repository | Checks | Purpose |
| --- | --- | --- | --- |
| `supporting/conformance-tests` | [`aws/aws-durable-execution-conformance-tests`](https://github.com/aws/aws-durable-execution-conformance-tests) | [![Build](https://github.com/aws/aws-durable-execution-conformance-tests/actions/workflows/ci.yml/badge.svg)](https://github.com/aws/aws-durable-execution-conformance-tests/actions/workflows/ci.yml) | Language-neutral requirements, runner, and reusable conformance workflows. |
| `supporting/ci` | [`aws/aws-durable-execution-ci`](https://github.com/aws/aws-durable-execution-ci) | [![AI PR Review](https://github.com/aws/aws-durable-execution-ci/actions/workflows/ai-pr-review.yml/badge.svg)](https://github.com/aws/aws-durable-execution-ci/actions/workflows/ai-pr-review.yml) | Shared GitHub Actions automation. |
| `supporting/docs` | [`aws/aws-durable-execution-docs`](https://github.com/aws/aws-durable-execution-docs) | [![Docs](https://github.com/aws/aws-durable-execution-docs/actions/workflows/docs.yml/badge.svg)](https://github.com/aws/aws-durable-execution-docs/actions/workflows/docs.yml) [![Quality](https://github.com/aws/aws-durable-execution-docs/actions/workflows/quality.yml/badge.svg)](https://github.com/aws/aws-durable-execution-docs/actions/workflows/quality.yml) | Cross-language AWS Lambda Durable Execution documentation. |

## Scope

A repository is included when it is public and active and either:

- implements the high-level Lambda Durable Execution handler and durable
  operation programming model; or
- provides shared conformance, CI, or documentation for those SDKs.

Generated low-level AWS Lambda service clients, application demos, deployment
frameworks, emulators, archived repositories, and empty proof-of-concept
repositories are outside this workspace's scope.

## Cross-repository contributions

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

## Clone

```bash
git clone --recurse-submodules https://github.com/zhongkechen/durable-execution-sdk.git
```

For an existing clone:

```bash
git submodule update --init --recursive
```

## Update

Pull the workspace and check out its recorded component revisions:

```bash
git pull --recurse-submodules
git submodule update --init --recursive
```

Changes must be committed in the component repository first. Commit the updated
submodule pointer in this repository after the component commit is available
from its remote.
