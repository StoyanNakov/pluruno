# Six-GPU OCI Parity Proof

Pluruno has completed a controlled container-runtime parity experiment across a six-GPU heterogeneous worker.

## Result

The accepted run executed **2048 deterministic parity checks** across six isolated GPU execution groups. All **2048/2048** checks matched the established host-native reference behavior exactly for the tested workload.

The experiment was designed as a shadow qualification: the containerized execution path was validated against the already accepted native path before any production ownership change.

## What this proves

For the tested Pluruno expert-runtime workload:

- the OCI/container execution path can reproduce the accepted native numerical behavior across all six tested GPUs;
- multiple GPU-isolated execution groups can be qualified independently on one physical participant;
- containerization itself did not introduce a numerical correctness regression in the measured workload;
- rollback/recovery preserved the pre-existing accepted runtime behavior after the qualification run.

## What this does not prove

This is a controlled-lab correctness result. It does **not** claim:

- universal portability to arbitrary GPU vendors or drivers;
- production readiness for public Internet peers;
- that every model or kernel is numerically identical across every container/runtime combination;
- that containerization has zero performance cost;
- completion of the universal participant/onboarding path.

## Engineering implication

The result supports Pluruno's participant-runtime direction: execution can be moved behind a portable OCI boundary without relaxing the project's correctness rule. The safe migration pattern is to qualify a shadow runtime against an already accepted reference, prove parity, and preserve rollback before changing ownership of the execution path.

For heterogeneous systems, this matters more than assuming that a container which merely starts successfully is equivalent to the native runtime.

## Status

**ACCEPTED experimental proof.**

These results describe the specific controlled Pluruno lab experiment and are not production performance or compatibility guarantees.
