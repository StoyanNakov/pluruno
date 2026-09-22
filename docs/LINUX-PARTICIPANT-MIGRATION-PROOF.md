# Linux Participant Migration Proof

## Status

**PROVEN in the current Pluruno Linux lab scope.**

This note records a completed result from the Pluruno validation backlog: distributed execution responsibilities were migrated across multiple existing Linux participants without changing the logical model semantics that the execution layer must preserve.

## What was demonstrated

The migration work exercised the participant/runtime boundary on more than one Linux peer rather than treating a single machine as a permanent execution role.

The accepted behavior was:

- participant capability and placement state can be re-established on a different eligible Linux peer;
- execution roles are assignments, not hard-coded machine identities;
- moving a physical assignment does not authorize changing the logical expert selected by the native model router;
- the control plane remains authoritative for desired placement while participants execute the assigned state;
- stale physical placement is not treated as valid simply because a previous participant once hosted it;
- post-migration execution must pass the same correctness boundary as the placement it replaces.

This is important for heterogeneous deployments because physical GPU capacity, host availability and network position can change independently of the model's logical routing decisions.

## Correctness boundary

Pluruno keeps two decisions separate:

1. **Logical routing** — the model selects the logical expert or model component required for the request.
2. **Physical placement** — Pluruno selects an eligible physical location that contains the exact required component.

Migration changes only the second decision. It must not silently substitute a different logical expert or otherwise alter model semantics.

## What this proof does not claim

This result is intentionally narrow. It does **not** claim:

- arbitrary migration between every hardware or operating-system combination;
- transparent live migration of in-flight GPU kernels;
- zero-cost migration;
- WAN-ready migration;
- Windows or macOS parity;
- an optimal global scheduler;
- universal automatic onboarding of previously unknown hardware.

Those are separate qualification or scheduling problems.

## Relationship to other Pluruno proofs

This migration result complements the existing capability-aware scheduling, desired-state reconciliation, exact-replica failover and Linux OCI participant-runtime proofs. Together they establish that Pluruno's execution topology is not defined by permanently fixed worker identities: physical placement can change while correctness remains bounded by the exact logical component requested by the model.

## Acceptance statement

Within the tested Linux lab boundary, multi-participant migration of execution responsibility is **accepted as proven**. Broader hardware, WAN and operating-system claims remain outside this proof.