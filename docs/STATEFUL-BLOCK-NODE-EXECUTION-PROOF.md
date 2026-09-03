# Stateful Block Node Execution Proof

Pluruno has experimentally validated **stateful Block Node execution** in the current controlled distributed MoE proof environment.

A Block Node is a deployment unit that hosts a sequential range of transformer layers rather than exposing every layer as a separate remote execution boundary.

## Why this matters

Fine-grained remote execution is useful for proving distribution and for placing work on small heterogeneous peers, but it can also create repeated synchronization and transport boundaries.

A Block Node tests a complementary deployment shape: keep several sequential layers within one execution location and return to the external execution path only at the block boundary.

## What was demonstrated

The accepted experimental foundation established that:

- a sequential range of transformer layers can execute inside one remote Block Node deployment unit;
- the Block Node can retain the execution state needed to progress through that range rather than behaving as a collection of unrelated one-layer calls;
- the external boundary preserves the expected model execution behavior for the current proof model;
- Block Nodes can coexist conceptually with finer-grained Expert Nodes and larger Expert Groups as different deployment choices for heterogeneous peers.

This is separate from the later Block Node sizing experiment, which compared multiple block sizes and measured their transport and generation-time behavior. The result here is the more basic execution proof: **stateful multi-layer remote execution is a working Pluruno deployment primitive in the controlled lab path**.

## Architectural implication

Pluruno does not need to force all remote capacity into one granularity.

A smaller peer may be useful for a fine-grained expert role, while a peer with sufficient compute, memory and locality may host a larger sequential block. The scheduler can therefore treat deployment shape as a placement choice instead of assuming that every model layer must cross the same remote boundary independently.

## Scope

This proof is intentionally narrow. It does **not** claim:

- that Block Nodes are always faster than fine-grained expert execution;
- a universal optimal block size;
- production performance over the public Internet;
- support for arbitrary transformer architectures;
- completed WAN placement or public-peer operation.

Those remain separate measurement and engineering problems.

For the measured block-size comparison, see [Block Node Sizing Proof](BLOCK-NODE-SIZING-PROOF.md).
