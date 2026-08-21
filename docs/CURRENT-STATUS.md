# Current Status

_Last updated: 2026-08-21_

Pluruno is currently an **experimental heterogeneous distributed AI execution prototype**.

The current proof path uses `allenai/OLMoE-1B-7B-0125-Instruct` to validate distributed execution semantics and topology behavior. The project architecture is intended to become manifest-driven and multi-model rather than remain tied to this model.

## Accepted experimental phases

### T0 — frozen baseline

Established the initial distributed inference control and correctness baseline.

### T1 — accepted

Separated the Global Control Plane from the Session / Model Executor hot path.

A controlled comparison showed that moving session/model execution off the weak control-plane CPU improved measured throughput from **1.797 tok/s to 3.379 tok/s**, an approximately **88% increase** in that experiment while preserving exact output.

### T2 — fully accepted

Demonstrated the broader distributed execution topology:

- two independent Session Executors under one control-plane concept;
- simultaneous exact sessions;
- distributed exact Expert Groups across several peers;
- local expert co-location;
- execution-domain failure isolation;
- preservation of exact replica behavior;
- topology/role observability.

In the concurrent-domain experiment, measured throughput was:

- Domain A: **5.643 tok/s**
- Domain B: **8.263 tok/s**

These are lab measurements, not production performance guarantees.

### T3 — fully accepted

T3 validated heterogeneous, capability-driven execution roles.

Accepted sub-phases include:

- **T3-A:** single exact Expert Node;
- **T3-B:** fine-grained multi-expert packing and one-GPU vs two-GPU packing;
- **T3-C:** capability-aware role promotion with selected/rejected decision explanations;
- **T3-D:** measured spillover-candidate comparison;
- **T3-E:** exact layer / DynamicCache / KV Block Node execution contract;
- **T3-F:** first real stateful multi-layer Block Node with exact boundary equivalence;
- **T3-G:** exact 1/2/4/8-layer Block Node sizing envelope;
- **T3-H:** objective-aware scheduler/topology observability in the production dashboard/API layer.

The current scheduler evidence shows that the best deployment can depend on the objective. For example, one measured Block Node configuration was advantageous for short/TTFT and bandwidth-locality objectives, while ordinary Expert Group execution remained preferable for decode-throughput objectives.

## What is proven today

- pretrained MoE experts can execute remotely on other physical machines;
- exact experts can be distributed across multiple peers;
- the model-native top-k decision remains authoritative in strict mode;
- Pluruno can choose among identical physical replicas without changing logical expert identity;
- request-level same-activation retry works after an in-flight replica failure;
- multiple independent execution domains can coexist;
- peers can carry multiple simultaneous roles;
- fine-grained Expert Nodes and larger Expert Groups both work;
- stateful multi-layer Block Node execution works;
- scheduler choices can be driven by measured capability and topology;
- selected and rejected scheduler candidates can be exposed for operator inspection.

## What is not yet claimed

Pluruno does **not** currently claim:

- a production public Internet network;
- safe execution with arbitrary untrusted peers;
- production Windows participation;
- automatic cross-region session migration;
- production multi-model manifests;
- a completed credit or token economy;
- blockchain integration;
- production-grade reputation or malicious-worker detection.

## Next phase: T4

The next major phase is **Windows + Linux mixed-swarm onboarding**.

The first step is discovery and qualification of the Windows peers before choosing a runtime strategy. The project will compare native Windows, WSL2, and container-based participation using real hardware/network evidence rather than assuming one path in advance.

The initial T4 work includes:

- persistent peer identity;
- GPU inventory and qualification;
- CPU/RAM capability;
- measured network capability;
- management/deployment path;
- artifact retrieval;
- role assignment;
- mixed-OS inference;
- heartbeat and recovery;
- dashboard visibility.

## After T4

Later validation will focus on WAN-like conditions, including controlled latency, bandwidth constraints, jitter, loss, stragglers, and peer disappearance, followed by the security work required before public peers are allowed.
