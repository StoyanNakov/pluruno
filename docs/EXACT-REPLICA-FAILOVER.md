# Exact-Replica Failover Proof

Pluruno's strict execution model separates **logical expert selection** from **physical replica selection**.

The pretrained model's native router/gate remains authoritative about which logical expert is required. Pluruno may choose among exact physical replicas of that same expert, but it must not silently substitute a different logical expert because a preferred deployment becomes unavailable.

## What was tested

The controlled Linux lab established a hot replica for a distributed expert deployment and then simulated failure of the active physical deployment while inference was running.

The failover path demonstrated that:

- the original logical expert identity was preserved;
- an exact physical replica could take over execution;
- the inference flow continued successfully after the simulated failure;
- failure handling did not require changing the model-native routing decision;
- the failed deployment was isolated from the rest of the distributed execution path.

## Correctness invariant

In strict pretrained-model mode:

```text
model-native router/gate
        decides
which logical expert is required

Pluruno
        decides
which exact physical replica executes it
```

A failed replica is therefore a **placement/failover event**, not permission to alter model semantics.

## Why this matters

Distributed inference across independent machines introduces more failure modes than a single-host deployment: a peer can disappear, a process can fail, or a network path can become unavailable.

If recovery changed the requested logical expert, the system could continue producing output while silently changing the model's intended computation. Pluruno treats that as incorrect behavior.

Exact-replica failover provides a safer foundation: the scheduler/runtime may recover from a physical failure while preserving the logical computation selected by the model.

## Scope of the proof

This result is a **controlled-lab experimental proof**, not a claim of production-grade Internet failover.

The proof establishes the execution invariant and demonstrates successful recovery between exact replicas in the current distributed MoE prototype. Public-WAN operation, arbitrary untrusted peers, and broader production fault tolerance remain separate future work.
