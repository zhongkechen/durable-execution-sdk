# Agent Guide for the Durable Execution SDK Workspace

This repository is a meta repository. It assembles independently versioned SDK,
conformance, CI, and documentation repositories as Git submodules; it does not
contain their source code directly.

## Start Here

1. Read the root [README.md](README.md) for the repository map, project scope,
   and community status definitions.
2. Run `git status --short --branch` at the workspace root.
3. Run `git submodule status --recursive` to see which components are
   initialized and whether their checkouts match the recorded revisions.
4. Before editing a component, read its local `AGENTS.md`, `CONTRIBUTING.md`,
   and other repository-specific instructions. The closest instructions to a
   file take precedence over this guide.

Initialize only the components needed for the task when possible:

```bash
git submodule update --init --recursive -- aws-maintained/js supporting/conformance-tests
```

Use `git submodule update --init --recursive` only when the task genuinely
requires the complete workspace.

## Concurrent Agent Isolation

When multiple AI agents are running different tasks in this workspace, each
agent must use its own isolated Git worktree and task-specific branch before
editing files. Do not let agents performing separate tasks share a working
tree, even when their expected changes do not overlap.

- Create temporary worktrees under the directory specified by `$TMPDIR`.
- Use one worktree and branch per agent task.
- Create the worktree in the repository that owns the files being changed,
  whether that is the workspace root or a component submodule.
- Do not switch branches, update submodules, reset, clean, or otherwise modify
  another agent's worktree.
- Exchange completed work through commits, patches, or cherry-picks rather than
  by editing another agent's working files.

## Put Changes in the Owning Repository

The workspace root owns only:

- `.gitmodules`
- the workspace `README.md` and other root-level metadata
- the Git commit pointers recorded for each submodule

Make implementation changes in the component that owns the behavior:

| Change | Owning path |
| --- | --- |
| SDK code, language-specific tests, examples, or conformance handlers | `aws-maintained/<language>` or the relevant `community/<project>` |
| Language-neutral requirements, runner behavior, or reusable conformance workflows | `supporting/conformance-tests` |
| Shared GitHub Actions automation | `supporting/ci` |
| Cross-language Durable Execution documentation | `supporting/docs` |
| Workspace membership, status, layout, or recorded component revisions | workspace root |

Do not copy component source into the root repository or make a root-only
workaround for behavior owned by a submodule.

## Shared SDK Engineering Rules

Apply these behavioral invariants when implementing or reviewing any SDK in
this workspace. The language-neutral requirements in
`supporting/conformance-tests` and the cross-language guidance in
`supporting/docs` are the canonical references. Public APIs should remain
idiomatic for their language; shared behavior does not require identical method
names or type shapes. SDK implementations, tests, documentation, and examples
must preserve and teach these invariants.

### Replay consistency

Durable handlers restart from the beginning after a suspension, retry, or
interruption. Completed operations must consume their checkpoints and return or
raise the stored outcome without re-running completed user code.

Given the same handler input and checkpoint history, an SDK must produce the
same durable execution:

- Call durable operations in the same sequence with stable operation types,
  names, parent contexts, and nesting.
- Keep operation names static and descriptive. Do not derive names from time,
  random values, mutable counters, or external state.
- Treat code outside durable operations as a pure function of handler inputs
  and checkpointed operation results.
- Put wall-clock reads, random values, UUID generation, external I/O, mutable
  global state, and runtime configuration that can change between invocations
  inside a step or other appropriate durable operation.
- Base branches, loop bounds, and concurrency topology only on deterministic
  inputs or checkpointed results.
- Start durable operations sequentially within one context. Use isolated child
  contexts for concurrent durable work, and do not use a parent context from a
  child-context scope.
- Return data from steps and assign the returned value outside the step. Do not
  depend on a step mutating a captured variable because completed step bodies
  are skipped during replay.
- Do not call `step`, `wait`, `invoke`, callbacks, or other durable operations
  from inside a step. Use a child context to group multiple operations.

Replay must preserve failures as well as successes. A completed failed
operation should raise the checkpointed error on replay without repeating the
operation's side effects. Preserve stable operation identity and error meaning
when changing internal implementations.

### Side effects and retries

- Put external side effects inside steps and keep one independently retryable
  side effect per step where practical.
- Assume at-least-once execution can repeat interrupted work. Make the operation
  idempotent, use a stable idempotency key, or select the SDK's at-most-once
  behavior when the capability exists.
- Do not describe at-most-once-per-retry as exactly-once execution. A configured
  retry policy can still start another attempt.
- Generate idempotency keys in a checkpointed operation so every replay and
  retry uses the same value.
- Let step failures reach the retry machinery. Retry transient failures with
  bounded backoff; fail errors known to be permanent without pointless
  retries.
- Do not use blocking sleeps to model durable delays. Use the SDK's durable
  wait or polling operations so the invocation can suspend.

### State, serialization, and compatibility

- Treat handler inputs, durable operation results, callback results, and child
  context results as persisted state.
- Verify that every checkpointed value round-trips through the configured
  serializer with its required type and meaning intact.
- Keep checkpoint payloads small. Persist identifiers or external-storage
  references instead of large API responses and aggregate payloads.
- Maintain backward compatibility for public APIs, serialized checkpoint data,
  operation identity, and replay behavior unless the change explicitly defines
  a migration or version boundary.
- Use the replay-aware context logger for orchestration logs. Ordinary logging
  outside steps may be emitted again on every replay.

### Cross-SDK changes and tests

- Define shared behavior in language-neutral terms first. Do not copy one
  language's incidental implementation details into every SDK.
- Exercise the real public API in conformance handlers. Do not hand-code around
  a missing SDK capability to make a requirement pass; report it as unsupported
  or `NOT_IMPLEMENTED` where the conformance framework allows.
- When behavior changes across SDKs, update the language-neutral requirement,
  applicable SDK handlers, cross-language documentation, and component tests in
  coordinated pull requests.
- Test both the initial invocation and at least one replay or resume path.
  Assert that completed work is skipped, stored results and failures are
  reproduced, branches remain stable, and side effects occur the expected
  number of times.
- Add focused coverage for interruption boundaries, retries, serialization,
  callbacks, and child-context concurrency when the change touches those
  behaviors.

## Working in a Submodule

Submodules are normally checked out at detached commits. Before committing
component changes, create or switch to a branch inside that submodule:

```bash
git -C aws-maintained/js status --short --branch
git -C aws-maintained/js switch -c codex/my-change
```

Then develop, test, commit, and open a pull request in that component's own
repository. Follow that repository's toolchain and agent instructions.

Keep these Git boundaries in mind:

- A root commit records a submodule commit pointer, not the files changed
  inside the submodule.
- `git status` at the root may summarize an entire component as one modified
  path. Inspect the component with `git -C <path> status` and
  `git -C <path> diff`.
- Do not run `git submodule update --force`, discard a component checkout, or
  move a submodule away from local work.
- Do not include unrelated component pointer changes in a workspace commit.
- Before recording a new pointer, make sure the component commit is committed
  and available from its remote. Other workspace users must be able to fetch
  it.

Record an intentional component revision at the workspace root with:

```bash
git add aws-maintained/js
git diff --cached --submodule=log
```

Never edit a gitlink as if it were a normal file.

## Cross-Repository Changes

For work spanning multiple repositories:

1. Identify every owning repository and any required merge order.
2. Create a branch and make the change separately in each affected submodule.
3. Run each component's focused tests and any relevant cross-repository or
   conformance validation with the intended commits checked out together.
4. Commit and open separate component pull requests, linking the related pull
   requests and documenting merge order.
5. After the component commits are remotely available, update their pointers
   in a separate workspace pull request.

Do not commit a workspace pointer to an unpublished local component commit.

## Validation

There is no single root build command. Validation belongs to the repositories
that changed:

- Run the formatter, linter, type checks, unit tests, and integration tests
  required by each affected component.
- Run conformance tests when behavior crosses SDK boundaries or changes a
  language-neutral requirement.
- For root documentation or `.gitmodules` changes, inspect the rendered
  Markdown, check links and paths, and verify submodule configuration with
  `git submodule status --recursive`.

Before finishing, review both levels of state:

```bash
git status --short --branch
git diff --submodule=log
git submodule foreach --recursive 'git status --short --branch'
```

Report which component tests ran and which were not available. If a community
submodule was intentionally left uninitialized, say so rather than treating
its absence as a successful validation.

## Common Task Shapes

### Root metadata only

Edit the root files, validate the Markdown or submodule configuration, and
commit only at the workspace root.

### Component implementation only

Work, test, and commit inside the owning submodule. Do not stage the changed
submodule pointer at the root unless the task also requests a workspace update.

### Component revision update

Check out the desired, remotely available component commit, stage only that
submodule path at the root, and verify the old and new commits with
`git diff --cached --submodule=log`.

### Adding or removing a component

Update `.gitmodules`, the gitlink, and the root `README.md` together. Keep the
AWS-maintained, community, and supporting classifications accurate, and
document the component's validation status without implying support or
endorsement beyond the evidence in its repository.
