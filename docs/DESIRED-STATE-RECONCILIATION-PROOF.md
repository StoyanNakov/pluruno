# Desired-State Reconciliation Proof

Pluruno has experimentally validated the control-plane foundation used to converge a participant from its current runtime state to an authoritative desired state.

This document records only the completed and accepted control/reconciliation behavior. It does not describe private deployment topology, credentials, authentication internals, or unfinished public-network onboarding work.

## What was proven

The accepted control-plane work established two complementary pieces:

1. an authoritative desired-state contract; and
2. persistent participant reconciliation semantics.

The controller publishes the complete intended deployment state for a participant rather than relying on a sequence of one-shot imperative commands. The participant compares that desired state with its observed runtime state, computes the required changes, applies them, and reports the resulting state back.

## Ordering and conflict behavior

Accepted semantics include a monotonically advancing assignment generation within the active control-plane epoch.

The tested rules distinguish between:

- an older assignment, which must not replace newer state;
- the same generation carrying the same effective desired state, which is idempotent;
- the same generation carrying conflicting desired state, which is treated as a conflict rather than silently applied; and
- a new control-plane epoch, which requires a full reconciliation against the new authoritative state.

These rules are important because retries, reconnects and process restarts are normal events in a distributed system.

## Restart and reconnect behavior

The reconciliation model is persistent rather than tied to one controller request. A participant can re-evaluate actual state after a restart or temporary disconnect and converge again toward the currently authoritative desired state.

This separates the questions:

- **What should be running?** — owned by the authoritative control plane.
- **What is running now?** — observed by the participant.
- **What changes are required?** — computed by reconciliation.

The result is a control model that supports repeatable recovery without treating every disconnect as a fresh ad-hoc deployment.

## Why this matters for heterogeneous peers

Pluruno assigns execution roles per deployment rather than permanently binding one physical machine to one role. A peer may therefore need to start, keep, replace or stop different deployment units as capabilities, locality and workload change.

Desired-state reconciliation provides the generic mechanism for those transitions without requiring machine-specific orchestration semantics.

## Control plane versus inference data plane

This proof concerns coordination state. It does not put the Global Control Plane into every model activation path.

Session execution and expert/block traffic can remain on the inference data plane while the control plane remains authoritative for deployment intent and system-level coordination.

That separation has also been demonstrated by the project's independent Session Executor work.

## Scope of the claim

This proof supports the following conservative claim:

> Pluruno has an accepted desired-state and reconciliation foundation that can recover and converge participant deployment state under retries, reconnects and restarts without relying on one-shot machine-specific commands.

It does **not** claim:

- production readiness for arbitrary public Internet participants;
- active-active control-plane high availability;
- completed automatic community onboarding;
- completed universal participant packaging;
- production WAN/NAT/relay operation; or
- zero-failure deployment transitions under every possible infrastructure fault.

Those remain separate engineering and qualification problems.
