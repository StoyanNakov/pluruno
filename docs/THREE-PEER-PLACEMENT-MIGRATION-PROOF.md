# Three-Peer Placement Migration Proof

## Status

**PROVEN for the tested Linux laboratory topology.**

This note records a narrower result than general participant portability: execution placement was exercised across multiple distinct Linux peers while preserving the logical model work being executed.

## What was demonstrated

The tested distributed MoE path used multiple Linux participants with different physical GPU placements. Expert execution responsibilities were assigned across those participants and the physical placement could change without changing the model's logical expert identity.

The important invariant is:

> The native model router selects the logical expert. Pluruno may change the physical peer or exact replica that executes that expert, but it must not silently substitute a different logical expert.

This result complements the broader participant-migration proof by recording that placement was not confined to a single source/destination pair: the laboratory execution path exercised a multi-peer topology and retained the same logical correctness boundary.

## Why this matters

A heterogeneous scheduler needs freedom to move work away from a particular machine because of capacity, locality, availability, or recovery decisions. That freedom is useful only if physical placement remains separate from model semantics.

The accepted behavior therefore separates:

- **logical identity** — the expert selected by the model;
- **physical placement** — the eligible peer/device executing that exact work;
- **recovery placement** — another eligible location for the same logical work when an exact replica exists.

## Acceptance boundary

This proof supports the following public claims:

- multi-peer Linux placement is functional in the tested topology;
- execution roles are not permanently bound to one machine identity;
- physical relocation does not authorize logical-expert substitution;
- placement and model routing are separate concerns.

It does **not** claim:

- live migration of an in-flight GPU kernel;
- zero-cost migration;
- arbitrary hardware or operating-system portability;
- WAN-ready placement or recovery;
- an optimal universal scheduler;
- that every possible role combination has been qualified.

## Relationship to other Pluruno proofs

This result should be read together with the exact-replica failover, distributed expert-group, capability-aware scheduling, and Linux participant migration proofs. Those documents establish different boundaries: failover correctness, distributed execution, scheduling foundations, and participant portability. This note isolates the multi-peer placement result so that it is not hidden inside broader architecture claims.
