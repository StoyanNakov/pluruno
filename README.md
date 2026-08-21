# Pluruno

**Many machines. One system.**

Pluruno is an experimental open research project for **heterogeneous distributed AI execution**. Its current proof path focuses on pretrained Mixture-of-Experts (MoE) inference, where independent peers with different GPUs, CPUs, memory, network links, operating systems, and uptime can contribute useful capacity without being forced into one fixed hardware profile.

> **Current status:** Linux heterogeneous prototype proven through the T3 topology phase. The next major phase is Windows + Linux mixed-swarm onboarding.
>
> Pluruno is **not yet a production public network** and is not ready for untrusted Internet peers.

**License:** [GNU Affero General Public License v3.0 only (AGPL-3.0-only)](LICENSE)

## Why Pluruno exists

Most distributed inference systems assume either a homogeneous cluster or a relatively fixed deployment shape. Pluruno is exploring a different model:

- peers may have very different capabilities;
- roles are assigned from measured capability, topology, locality, and demand;
- one physical peer may perform several roles at the same time;
- the global control plane should not sit in every inference activation path;
- model-native routing remains authoritative in strict execution mode;
- exact replicas provide failure tolerance without silently changing the requested logical expert;
- operator visibility and scheduler reasoning are part of the architecture, not an afterthought.

## Core execution roles

Pluruno currently models several distinct execution roles:

- **Session / Model Executor** — owns the non-expert transformer path, attention, KV cache, native MoE gate, generation loop, and expert RPC orchestration.
- **Expert Node** — hosts one or a small number of exact experts.
- **Expert Group** — hosts a larger expert set, potentially across many layers.
- **Block Node** — hosts sequential transformer blocks to reduce repeated network synchronization and improve locality.
- **Exact Replica** — provides same-expert retry and redundancy.

A peer can hold multiple roles simultaneously when that reduces latency or improves resource use.

## Architecture direction

```text
                     Global Control Plane
              registry / health / placement
                 topology / observability
                          |
               choose execution domain
                          |
             +------------+------------+
             |                         |
      Session Executor A        Session Executor B
          /    |    \                /    |    \
         /     |     \              /     |     \
     local   nearby   replica     local  nearby  replica
     roles    roles     roles      roles   roles    roles
```

The intended Internet architecture is based on **session/regional execution domains and nearby exact deployments**, rather than bouncing every activation through one global router.

## Verified experimental milestones

The current Linux lab has already demonstrated:

- exact pretrained MoE experts physically distributed across multiple peers;
- native top-k routing preserved while physical replica selection remains a Pluruno decision;
- Global Control Plane separated from the Session / Model Executor hot path;
- two independent Session Executors operating simultaneously under one control plane;
- physically distributed exact Expert Groups;
- local expert co-location on an executor;
- exact request-level replica retry after an in-flight failure;
- independent failure isolation between execution domains;
- single-expert **Expert Node** execution;
- fine-grained multi-expert packing across one or more GPUs;
- capability-aware role promotion with selected/rejected candidate reasoning;
- real stateful **Block Node** execution with exact boundary equivalence;
- exact 1-, 2-, 4-, and 8-layer Block Node experiments;
- objective-aware scheduler decisions exposed through dashboard/API observability.

The full T2 and T3 experimental topology phases are accepted in the internal test program.

## Selected measurements

These numbers are experimental lab results, not production performance claims.

| Experiment | Result |
|---|---:|
| Early centralized control/executor baseline | 1.797 tok/s |
| Session Executor moved off the weak control-plane CPU | 3.379 tok/s |
| Improvement in that controlled comparison | +88.0% throughput |
| Concurrent execution domain A | 5.643 tok/s |
| Concurrent execution domain B | 8.263 tok/s |

One of the main conclusions so far is that **worker GPU math alone is not the dominant problem**. CPU hot paths, network topology, repeated synchronization, locality, and role placement can matter just as much or more.

## Correctness first

In the current strict pretrained-model mode:

```text
native pretrained model gate
        decides
which logical expert is required

Pluruno
        decides
which exact physical replica executes it
```

Pluruno does not silently substitute a different logical expert simply because the preferred peer failed. Same-request retry uses an identical replica while retaining the activation payload until the request has an outcome.

## Current roadmap

**Next:** Windows + Linux mixed-swarm onboarding.

The first Windows phase will measure and freeze real hardware, GPU, CPU/RAM, network, and runtime capability before choosing between native Windows, WSL2, or container-based participation.

Later research areas include:

- controlled WAN latency/bandwidth shaping;
- regional/session execution cells;
- expert affinity and locality-aware placement;
- multi-model manifests and immutable model revisions;
- authenticated peer identity and signed manifests;
- encrypted transport and public-peer hardening;
- malicious-worker detection and sandbox boundaries;
- contribution accounting, credits, and reputation.

The credit/economy layer is part of the longer-term vision and is **not implemented as the current priority**.

See:

- [Architecture](docs/ARCHITECTURE.md)
- [Current Status](docs/CURRENT-STATUS.md)
- [Roadmap](docs/ROADMAP.md)
- [Security](SECURITY.md)
- [Contributing](CONTRIBUTING.md)

## Proof model

The primary current proof model is:

`allenai/OLMoE-1B-7B-0125-Instruct`

The architecture is intended to become manifest-driven and multi-model rather than remain tied to one proof model.

## Project stage

Pluruno is currently an **experimental research prototype**. Interfaces, protocols, deployment roles, and architecture may still change as mixed-OS and WAN validation progresses.

## Licensing

Pluruno is licensed under the **GNU Affero General Public License v3.0 only** (`AGPL-3.0-only`). See [LICENSE](LICENSE).
