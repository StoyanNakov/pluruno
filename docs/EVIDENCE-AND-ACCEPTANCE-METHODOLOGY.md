# Evidence and Acceptance Methodology

Pluruno treats a distributed-inference capability as accepted only after the claimed behavior has been exercised and the observed result supports the claim. Architecture sketches, planned features, successful process startup, or an isolated happy-path log are not sufficient by themselves.

This document records the methodology already used by the project to separate proven behavior from work that is still experimental.

## Claim first, evidence second

Each test starts with a bounded claim. Examples include exact remote expert execution, exact-replica failover, a complete distributed MoE forward pass, autoregressive generation, multi-GPU runtime parity, or a measured transport comparison.

The evidence is evaluated only against that claim. A successful narrow test is not promoted into a broader statement such as universal hardware support, WAN readiness, or optimal scheduling.

## Correctness before performance

For distributed MoE execution, logical model semantics remain authoritative. The model router selects the logical expert. Physical placement may choose where an exact instance of that expert executes, but placement must not silently substitute a different logical expert.

Correctness checks therefore precede throughput optimization. Performance results are meaningful only after the execution path under measurement has passed the relevant correctness gate.

## Reproducible boundaries

Accepted experiments record the execution boundary being tested: local versus remote work, placement shape, transport mode, and the metric being compared. Where practical, A/B measurements keep the workload fixed while changing one relevant dimension.

This is why Pluruno publishes measurements such as Session Executor placement and FP16 versus Q8_0 transport as bounded observations rather than universal performance claims.

## Negative results are evidence

A technically valid experiment can be useful even when it rejects an optimization. For example, a transport or batching idea that functions correctly but makes the measured workload slower is retained as evidence against enabling that optimization by default.

Rejected optimizations are not converted into positive claims merely because the mechanism worked.

## Failure and recovery are separate gates

Normal execution success does not prove recovery behavior. Failover, restart, reconnect, stale-state handling, and reconciliation are tested as separate properties.

Likewise, successful failover does not authorize substitution of a different logical MoE expert. Recovery must preserve the correctness contract.

## Conservative publication rule

Public status uses a small vocabulary deliberately:

- **PROVEN** means the bounded behavior has direct accepted evidence.
- **PROVEN FOUNDATION** means the underlying mechanism is accepted, while broader policy or optimization remains incomplete.
- **UNDER DEVELOPMENT**, **DEFERRED**, **PLANNED**, and **NOT YET ACCEPTED/IMPLEMENTED** are not presented as completed capabilities.

A broader claim is withheld until its own acceptance evidence exists, even when several prerequisite components have already passed.

## Why this matters

Distributed inference combines model semantics, placement, networking, device behavior, and failure handling. A system can appear healthy while violating one of those layers. Pluruno therefore keeps correctness, performance, recovery, and portability as distinct acceptance dimensions.

The result is intentionally conservative: public claims should describe what the evidence demonstrates, not what the architecture is expected to support eventually.
