# Durable Execution SDK Workspace

This repository provides a single workspace for the active public AWS Durable
Execution repositories. Each component remains independently versioned and is
included here as a Git submodule pinned to a specific commit.

## Repositories

| Path | Repository |
| --- | --- |
| `repos/sdk-java` | `aws/aws-durable-execution-sdk-java` |
| `repos/sdk-js` | `aws/aws-durable-execution-sdk-js` |
| `repos/sdk-python` | `aws/aws-durable-execution-sdk-python` |
| `repos/conformance-tests` | `aws/aws-durable-execution-conformance-tests` |
| `repos/ci` | `aws/aws-durable-execution-ci` |
| `repos/docs` | `aws/aws-durable-execution-docs` |

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
