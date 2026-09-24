# Heterogeneous Link Topology Proof

Pluruno is designed for heterogeneous participants rather than assuming that every peer has the same accelerator, host CPU, or network path. The accepted lab evidence includes Linux participants connected through materially different link classes, including an intentionally constrained approximately 100 Mb/s path alongside approximately 1 Gb/s LAN paths.

This document records the narrow result that has already been exercised: the distributed execution model can keep physical placement and transport topology explicit while preserving model semantics across unequal participant paths.

## Proven boundary

The tested topology established that:

- participant links do not need to have identical bandwidth;
- physical execution placement can span peers with different compute and network characteristics;
- the Session Executor and remote Expert/Expert Group path remain distinct responsibilities;
- the pretrained model router remains authoritative for logical expert selection;
- Pluruno may choose the physical location of the exact selected expert, but must not silently substitute a different logical expert because another path is faster;
- network cost must be measured separately from executor-local work and remote compute.

A deliberately slower link was useful precisely because it exposed behavior that would be hidden in a uniform fast-LAN testbed.

## Why link heterogeneity matters

Distributed inference is not governed by nominal bandwidth alone. End-to-end token latency can include executor-side work, connection and request overhead, activation transfer, remote compute, queueing, and synchronization.

Pluruno therefore treats topology as an input to placement decisions rather than assuming that an available peer is automatically a useful peer for every execution unit.

This is consistent with the existing transport measurements: reducing external request chatter and preserving persistent connections can matter substantially even when remote accelerator compute itself is relatively small.

## Placement implication

The accepted design rule is:

> preserve the model's logical routing decision first; then choose among valid physical placements using measured participant and path characteristics.

This allows future scheduling policy to compare locality, measured link behavior, compute capability, load, and exact-replica availability without changing pretrained model semantics.

## What this proof does not establish

This proof does **not** claim:

- that approximately 100 Mb/s is sufficient for every model or placement shape;
- that 1 Gb/s LAN is always faster than local execution;
- that current placement policy is globally optimal;
- that Internet/WAN inference is production-ready;
- that bandwidth alone is enough to predict placement cost;
- that arbitrary hardware or operating systems are already qualified.

WAN paths introduce additional RTT, jitter, loss, reconnect, and trust constraints and require separate acceptance evidence.

## Status

**Heterogeneous LAN link topology: PROVEN for the tested Linux lab boundary.**

**Bandwidth-aware universal scheduling: NOT YET CLAIMED.**

**Internet-tolerant production inference: NOT YET CLAIMED.**

The result supports Pluruno's current direction: keep topology and capability measurements explicit, preserve exact logical execution semantics, and make physical placement decisions from observed rather than assumed costs.
