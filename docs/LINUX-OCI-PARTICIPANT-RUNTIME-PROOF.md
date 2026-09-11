# Linux OCI Participant Runtime Proof

Pluruno has experimentally validated an **OCI/container participant runtime path on multiple Linux participants** in the controlled research environment.

This result is intentionally narrower than a claim of a universal installer or production public-peer runtime. It records an accepted migration result that is already reflected in the project's current status.

## What was demonstrated

The accepted Linux runtime work established that:

- three Linux participants were migrated through accepted OCI/container execution paths;
- distributed execution continued behind the container runtime boundary on those tested participants;
- the migration preserved the project's requirement to qualify runtime behavior rather than treating container startup alone as proof of correctness;
- the participant/runtime direction can use an OCI boundary without requiring the Global Control Plane to become part of the inference hot path;
- a separate six-GPU heterogeneous participant later passed a stronger shadow-parity qualification, reported independently in `SIX-GPU-OCI-PARITY-PROOF.md`.

## Why this matters

A heterogeneous distributed system needs a runtime boundary that is portable enough to deploy repeatedly while still allowing each participant to expose its real hardware capability.

For Pluruno, OCI/containerization is useful as an execution boundary, but it is not considered evidence by itself. A participant remains qualified only when the tested runtime preserves the required execution behavior for that environment.

## Established boundary

This proof does **not** claim:

- a completed universal participant package;
- automatic onboarding for arbitrary machines;
- production Windows or macOS participation;
- production AMD accelerator support;
- universal compatibility across all GPU drivers, runtimes, kernels, or models;
- production WAN or public Internet readiness;
- zero performance overhead from containerization.

Those remain separate engineering stages.

## Relationship to the six-GPU parity proof

The multi-participant Linux OCI migration and the six-GPU parity experiment establish related but different results.

This document records that the OCI participant path was accepted on multiple Linux participants. The six-GPU proof separately demonstrates exact parity checks across six isolated GPU execution groups on another heterogeneous participant.

Together they support the same engineering principle: **portable packaging must be qualified against accepted execution behavior, not assumed correct merely because the container launches successfully.**

## Status

**ACCEPTED experimental foundation on the tested Linux participants.**

These results describe controlled Pluruno lab validation and are not production portability, performance, or Internet-readiness guarantees.
