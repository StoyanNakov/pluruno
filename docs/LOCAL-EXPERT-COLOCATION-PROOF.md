# Local Expert Co-location Proof

Pluruno has experimentally validated **local expert co-location** as part of its heterogeneous placement model.

This result is deliberately narrower than a claim of a finished global scheduler. It records that a physical peer can combine the Session / Model Executor role with useful exact model capacity on the same machine, so some selected expert work can remain local instead of requiring a network hop.

## Question

Can Pluruno preserve the pretrained model's native logical expert selection while placing an exact physical deployment of that expert on the same peer as the active Session Executor?

## Accepted result

The controlled Linux research prototype has validated local expert co-location among its completed experimental foundations.

Pluruno assigns roles per model/deployment rather than permanently per machine. A peer can therefore act as a Session Executor and also host exact expert capacity for the same model. When the pretrained router selects a logical expert that has an eligible exact local deployment, that work can use the local deployment instead of forcing the activation through a remote peer.

The logical expert identity is unchanged. Co-location changes **where the exact requested deployment executes**, not which logical expert the pretrained model selected.

## Why this matters

Distributed inference cost is not determined by accelerator throughput alone. A remote peer can have stronger compute while still being a worse execution target when network transfer, synchronization, queueing, or locality costs dominate.

Local co-location therefore gives the scheduler another valid placement option:

```text
Session Executor
      |
      +-- exact local expert deployment
      |
      +-- exact remote expert deployment
```

The scheduler can consider measured capability, topology, locality, current load, and replica availability while preserving the strict logical-expert correctness rule.

## Relationship to other Pluruno proofs

This result is complementary to, but distinct from, other published proofs:

- **Distributed MoE execution** proves that selected experts can execute on remote physical peers.
- **Exact-replica failover** proves that a failed physical deployment can retry against an identical replica without substituting another logical expert.
- **Capability-aware scheduling** establishes that heterogeneous placement decisions can use measured machine and topology characteristics.
- **Local expert co-location** establishes that useful exact expert capacity may also reside with the active Session Executor and avoid a network hop for that eligible work.

Together these results support a placement model that can mix local and remote exact deployments rather than forcing every expert activation into one fixed topology.

## What is not claimed

This proof does **not** claim:

- that local placement is always faster than remote placement;
- that Pluruno has completed a globally optimal production scheduler;
- production WAN or public-Internet performance;
- universal support for arbitrary MoE architectures;
- automatic multi-model residency management;
- a completed universal participant/onboarding package.

Those remain separate engineering and measurement stages.

## Supported conclusion

The supported conclusion is narrow: **Pluruno has experimentally validated local expert co-location as a valid exact placement option within its heterogeneous execution architecture, while preserving the pretrained model's logical expert selection.**

## Publication boundary

This note intentionally omits machine-specific addresses, hostnames, credentials, private runtime state, local filesystem paths, raw operational evidence, and deployment-sensitive security details.
