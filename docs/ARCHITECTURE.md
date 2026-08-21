# Pluruno Architecture

This document describes the **public architectural direction** of Pluruno. It intentionally omits private lab addresses, credentials, host-specific paths, service names, and other deployment-sensitive details.

## Design goal

Pluruno explores how heterogeneous community-owned machines can participate in distributed AI execution without requiring identical hardware or one permanent role per machine.

The core design principle is:

> **Adapt the role to the machine, not the machine to the role.**

A participant may contribute GPU compute, CPU capability, memory, network locality, storage, or combinations of these resources.

## Control plane and data plane

Pluruno separates global coordination from session execution.

### Global Control Plane

The control plane is responsible for system-level coordination such as:

- peer registry and persistent identity;
- hardware/capability metadata;
- model and deployment manifests;
- desired state and assignments;
- health and telemetry aggregation;
- session placement;
- topology/cell decisions;
- scheduler observability;
- later reputation and contribution accounting.

The Global Control Plane should **not automatically be part of the hot activation path** for every model layer and token.

### Session / Model Executor

A Session Executor owns the session-specific execution path, including:

- non-expert transformer computation;
- attention;
- KV cache;
- the pretrained model's native MoE routing/gating decision;
- activation packing;
- expert RPC orchestration;
- expert output combination;
- generation loop.

Sessions can be pinned to an executor or execution domain chosen for locality and capability.

## Deployment roles

### Expert Node

Hosts one or a small number of exact experts. This role is useful for smaller or slower peers, fine-grained placement, targeted replicas, and spillover capacity.

### Expert Group

Hosts a larger set of exact experts, potentially across many or all MoE layers. This is useful when a peer has enough VRAM and compute capacity to efficiently serve a broader working set.

### Block Node

Hosts a sequential range of transformer layers. The purpose is to reduce repeated synchronization across the network by moving more sequential work into the same execution location.

Pluruno has experimentally validated stateful Block Node execution while preserving exact boundary behavior in the current proof model.

### Exact Replica

Hosts another physical copy of the **same logical deployment unit**. Replicas support same-request retry and failure tolerance without silently substituting a different logical expert.

## Multi-role peers

Roles are assigned per model/deployment, not permanently to the physical machine.

A peer may therefore be, for example:

```text
Peer A
├── Session Executor for Model A
├── Expert Group for Model A
└── Exact Replica for Model B
```

or:

```text
Peer B
├── Expert Node for Model A
└── Block Node for Model C
```

This matters for locality. If a Session Executor also hosts useful model capacity locally, some execution can avoid network traffic entirely.

## Strict pretrained-model semantics

In the current strict mode, the pretrained model remains authoritative about **which logical expert is required**.

```text
Native model gate
      |
      v
logical expert identity
      |
      v
Pluruno chooses an exact physical replica
```

Pluruno may choose where the requested logical expert runs, but does not silently replace it with a different logical expert because a machine is unavailable.

## Failure model

The current architecture preserves the activation payload until a request has an outcome. If a physical replica fails after work has already been dispatched, the same request can retry against an identical replica.

Conceptually:

```text
required logical expert
        |
        v
primary physical replica
        |
      failure
        |
        v
same activation payload
        |
        v
identical replica
        |
        v
request continues
```

Failure of one Session Executor should also be isolated from other independent execution domains.

## Heterogeneous scheduling

Physical placement is intended to consider measured properties such as:

- exact model/deployment identity;
- latency between executor and peer;
- measured bandwidth;
- GPU capability and throughput;
- CPU capability;
- available VRAM and RAM;
- current queue/load;
- topology and locality;
- replica availability;
- reliability/uptime;
- residency state;
- later contribution/reputation signals.

Scheduler decisions should remain inspectable: the operator should be able to see not only what was selected, but why alternatives were rejected.

## Regional / execution-cell direction

A naive Internet-scale design that sends every MoE layer activation through one global router would accumulate sequential network latency.

The intended direction is instead:

```text
                 Global Control Plane
                         |
                  choose execution cell
                         |
             +-----------+-----------+
             |                       |
      Regional Executor A      Regional Executor B
        /      |      \          /      |      \
     local   nearby   replica  local   nearby   replica
```

The global layer coordinates placement, health, and accounting. The hot data path should stay as local as practical to the active Session Executor.

## Multi-model direction

Pluruno is not intended to remain tied to a single proof model. Deployment identities should be namespaced by at least:

```text
model + immutable revision + role + layer/expert/block + content hash
```

A session belongs to exactly one model and revision. KV cache, hidden state, and deployment units must never be mixed across incompatible model revisions.

## Residency model

Future multi-model scheduling is expected to distinguish:

- **HOT** — weights already in accelerator memory;
- **WARM** — weights available in system memory;
- **COLD** — weights available on local storage.

The scheduler may move deployments between these states based on demand while avoiding excessive churn.

## Observability as architecture

Pluruno treats observability as part of the product architecture. A distributed data plane must not become an invisible data plane.

The system direction includes visibility into:

- peers and persistent identities;
- hardware capability;
- roles;
- model deployments;
- assignment generations;
- runtime state;
- failures and recovery;
- retries/failover;
- scheduler candidate selection and rejection reasoning.
