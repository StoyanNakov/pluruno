# Current Status

_Last updated: 2026-08-24_

Pluruno is an **experimental heterogeneous distributed AI execution platform**. The current controlled-lab proof path uses `allenai/OLMoE-1B-7B-0125-Instruct` to validate distributed MoE execution, heterogeneous role placement, failure handling, and participant/runtime architecture.

Pluruno is **not yet a production public Internet network**.

## Current maturity

| Area | Status |
|---|---|
| Distributed MoE execution | **PROVEN** |
| Remote expert execution | **PROVEN** |
| Exact-replica failover | **PROVEN** |
| Session Executor | **PROVEN** |
| Expert Node / Expert Group / Block Node | **PROVEN** |
| Protocol v2 | **PROVEN** |
| Capability-aware scheduling | **PROVEN FOUNDATION** |
| Authoritative Control Plane | **PROVEN** |
| Desired-state reconciliation | **PROVEN** |
| Linux OCI participant runtime | **PROVEN on current lab nodes** |
| Automatic participant onboarding | **UNDER DEVELOPMENT** |
| Measured qualification | **UNDER DEVELOPMENT** |
| Universal participant package | **NOT YET ACCEPTED** |
| WAN / public community network | **NOT READY** |
| Windows participant | **DEFERRED** |
| macOS participant | **PLANNED** |
| AMD production support | **NOT YET IMPLEMENTED** |
| Credits / tokens | **NOT YET IMPLEMENTED** |
| HA Control Plane | **NOT YET IMPLEMENTED** |

## What has been demonstrated

The controlled Linux lab has demonstrated:

- pretrained MoE experts executing remotely on other physical machines;
- a full 16-layer forward using remote experts;
- autoregressive generation using distributed expert execution;
- single-expert numerical correctness with observed difference below `1e-6`;
- exact-replica failover while preserving the requested logical expert;
- multiple independent Session Executors under one control-plane architecture;
- Expert Node, Expert Group, and stateful multi-layer Block Node execution;
- capability-aware role selection and scheduler observability;
- participant desired-state and reconciliation semantics;
- an authoritative Control Plane separated from the inference hot path;
- OCI/container migration of three Linux workers;
- six-GPU OCI shadow parity on a fourth heterogeneous worker, with **2048/2048 exact parity checks**.

These are experimental lab results, not production performance guarantees.

## Selected experimental measurements

| Experiment | Result |
|---|---:|
| Early centralized control/executor baseline | 1.797 tok/s |
| Session Executor moved off the weak control-plane CPU | 3.379 tok/s |
| Improvement in that controlled comparison | +88.0% throughput |
| Concurrent execution domain A | 5.643 tok/s |
| Concurrent execution domain B | 8.263 tok/s |

Protocol v2 transport experiments also measured an FP16 baseline of approximately **1.904 tok/s**. A Q8_0 transport experiment reached approximately **1.777 tok/s** in the measured run, so FP16 remains the current default while lossy transport remains opt-in research.

## Correctness rule

In strict pretrained-model execution, the model's native router/gate remains authoritative about **which logical expert** is required. Pluruno decides **which exact physical replica** executes that expert.

A failed physical replica therefore does not justify silently substituting a different logical expert.

## Heterogeneous execution

Pluruno does not assume identical peers. The lab intentionally includes different CPU/GPU capacities and materially different network links, including a worker constrained to roughly 100 Mb/s while other paths are around 1 Gb/s.

This is important to the project: role placement and scheduling should be based on measured capability, topology, locality, and workload rather than assuming a homogeneous cluster.

## Linux OCI progress

The Linux participant/runtime migration has progressed through accepted OCI paths on worker1, worker2, and worker3. A six-GPU fourth worker has also passed protected-baseline, rootless GPU-path, and six-GPU shadow-parity stages.

The remaining work on that worker is intentionally gated on the generic participant/onboarding path rather than being completed with a machine-specific workaround.

## What is not yet claimed

Pluruno does **not** currently claim:

- production readiness for arbitrary public Internet peers;
- a completed universal installer/participant package;
- production WAN/NAT/relay operation;
- production Windows or macOS participation;
- production AMD accelerator support;
- a completed credit/token/reputation economy;
- active-active highly available control-plane operation.

## Development principle

A terminal `PASS` is not by itself treated as project acceptance. Accepted work is reviewed against evidence, invariants, rollback behavior, and publication safety before being treated as proven.

The repository will continue to publish completed, reviewable engineering results as they are accepted rather than presenting planned work as finished.