# Control Plane / Session Executor Separation Proof

_Status: verified experimental foundation_

Pluruno separates global coordination from the session-specific inference hot path. The Global Control Plane remains authoritative for system-level coordination, while a Session / Model Executor owns the active model execution loop.

This document records the already accepted experimental result without extending it into a production claim.

## What was demonstrated

In the controlled Linux proof environment, Pluruno demonstrated that:

- the Global Control Plane can remain responsible for coordination and placement without executing every inference activation;
- a Session Executor can own the non-expert transformer path, attention, KV cache, native MoE routing decision, expert RPC orchestration, output combination, and generation loop;
- Session Executors can operate as independent execution domains under one control-plane architecture;
- moving the active Session Executor away from a weaker control-plane CPU materially improved throughput in the measured comparison;
- failure of one Session Executor can be isolated from another independent execution domain.

## Measured comparison

One controlled comparison measured:

| Placement | Throughput |
|---|---:|
| Early centralized control/executor baseline | 1.797 tok/s |
| Session Executor moved off the weaker control-plane CPU | 3.379 tok/s |
| Measured improvement | +88.0% |

These values describe that specific experimental topology and workload. They are not universal performance guarantees.

## Architectural implication

The result supports a central Pluruno design rule:

> Global coordination does not require the Global Control Plane to sit in every token or layer activation path.

The control plane may decide placement, desired state, health policy, and execution-domain assignment. Once a session is assigned, the active Session Executor can drive the model-specific hot path directly against the relevant local or remote execution roles.

This reduces unnecessary coupling between cluster-wide coordination work and latency-sensitive inference work.

## Correctness boundary

Separating the control plane from the hot path does not change strict pretrained-model semantics.

The pretrained model's native router/gate remains authoritative for the required logical expert. Pluruno may choose an exact physical replica for that logical expert, but the control-plane separation does not authorize logical-expert substitution.

## What this does not prove

This result does not establish:

- production readiness for arbitrary Internet peers;
- globally optimal executor placement;
- production WAN/NAT/relay operation;
- a highly available active-active control plane;
- universal performance improvement from every possible executor move;
- completion of automatic participant onboarding or mixed-OS participation.

Those remain separate engineering and qualification problems.

## Why this matters

Distributed inference can become bottlenecked by CPU orchestration, synchronization, topology, and placement even when worker GPU compute is available. Keeping global coordination outside the per-token hot path provides a cleaner basis for locality-aware execution domains and future regional/session cells while preserving one authoritative coordination layer.
