# Multi-Role Peer Proof

## Status

**PROVEN for the current Linux laboratory topology.**

This note records an accepted Pluruno result: a physical participant is not permanently defined by a single cluster-wide role. Execution responsibilities can be assigned according to the work being performed and the capabilities available on that participant.

This is a statement about the proven execution model, not a claim that every possible role combination, operating system, accelerator, or WAN topology has been qualified.

## What was demonstrated

Pluruno has exercised distinct execution responsibilities in the same distributed design:

- **Session Executor** responsibility for model/session orchestration and token-generation progress.
- **Expert execution** responsibility for exact routed MoE expert work.
- **Expert Group / Expert Node** placement that can pack multiple exact expert instances behind a participant execution boundary.
- **Block Node** execution for stateful contiguous multi-layer work.
- **Control-plane participation** remains separate from inference dataplane execution, so assigning an inference responsibility does not make the participant the authoritative controller.

The important result is that these are placement/execution responsibilities rather than permanently hard-coded machine classes.

## Why this matters

Heterogeneous clusters rarely have one universally best machine type. CPU performance, accelerator memory, accelerator count, link quality, and locality can make different participants suitable for different work.

The accepted Session Executor placement experiment is a concrete example: moving the same distributed workload to a stronger executor changed end-to-end performance substantially while the remote expert path remained the same. That demonstrates that executor placement itself is a meaningful scheduling dimension rather than a fixed property of a designated machine.

Likewise, accepted Expert Node packing and stateful Block Node experiments show that physical placement granularity can change without changing the model's logical expert identity or native routing semantics.

## Invariants preserved

Multi-role placement does **not** relax Pluruno correctness rules:

1. The native model router selects the logical expert.
2. Placement selects where that exact logical work executes.
3. Retry/failover may select an exact replica, but must not silently substitute a different logical expert.
4. Physical packing or colocation does not redefine model semantics.
5. Control-plane authority remains distinct from dataplane execution responsibility.

## What this proof does not claim

This result does not establish:

- a globally optimal scheduler;
- arbitrary simultaneous role combinations on every participant;
- universal participant onboarding;
- Windows or macOS parity;
- public-Internet readiness;
- automatic economic or credit-based placement;
- that every workload benefits from combining roles on one participant.

Those require separate qualification.

## Design consequence

Pluruno should continue to describe participants by capabilities and assign execution responsibilities from desired state instead of encoding permanent machine classes into the inference architecture.

This keeps heterogeneous placement decisions measurable and reversible while preserving exact model semantics.

## Related accepted evidence

- `SESSION-EXECUTOR-PLACEMENT-PROOF.md`
- `EXPERT-NODE-PACKING-PROOF.md`
- `STATEFUL-BLOCK-NODE-EXECUTION-PROOF.md`
- `CONTROL-PLANE-SEPARATION-PROOF.md`
- `CAPABILITY-AWARE-SCHEDULING-FOUNDATION.md`
