# Distributed Expert Groups Proof

Pluruno has experimentally validated **physically distributed Expert Groups** as one of its execution-unit options for heterogeneous Mixture-of-Experts inference.

This result is intentionally narrower than a claim of a production Internet-scale expert fabric. It establishes that a pretrained model can keep its native logical expert selection while the exact expert capacity required by that routing decision is hosted across multiple physical peers.

## What was demonstrated

In the controlled Linux research environment, Pluruno demonstrated:

- pretrained MoE expert computation executing remotely on other physical machines;
- expert capacity distributed across multiple physical peers;
- Expert Groups as a valid deployment role alongside individual Expert Nodes and stateful Block Nodes;
- complete 16-layer forward execution through the distributed expert path;
- autoregressive generation using distributed expert execution;
- preservation of the strict logical-routing rule: the pretrained model chooses the logical expert, while Pluruno chooses an exact physical deployment of that expert.

These results establish that expert capacity does not need to reside on one homogeneous machine or one accelerator to participate in the accepted proof path.

## Deployment semantics

An Expert Group hosts a broader set of exact experts than a small Expert Node. The grouping is a physical placement decision; it does not redefine the model's logical expert identities.

Conceptually:

```text
native model gate
      |
      v
required logical experts
      |
      +----> exact capacity on Expert Group A
      |
      +----> exact capacity on Expert Group B
      |
      +----> exact capacity on another valid deployment
```

The model-native routing result remains authoritative. Distribution changes **where** exact expert computation runs, not **which** logical experts the pretrained model requested.

## Why this matters

Expert Groups give Pluruno a deployment unit between two useful extremes:

- a small Expert Node, suitable for fine-grained placement and smaller contributors;
- a stateful Block Node, which keeps a sequential range of transformer work together.

A broader expert working set can be placed on a peer with suitable accelerator memory and compute capacity, while other expert capacity remains on other peers. This supports heterogeneous placement without requiring every participant to host the same amount or type of model state.

## Relationship to scheduling

The accepted result supports capability-aware role placement. A scheduler may treat an Expert Group as one available physical deployment of exact model capacity and choose among compatible placements using measured capability, topology, locality, load, and replica availability.

This does not imply that larger Expert Groups are always faster or that one grouping strategy is globally optimal. The preferred packing depends on the workload and the participating machines.

## Established boundary

This proof does **not** claim:

- production readiness for arbitrary public Internet peers;
- universal support for every MoE architecture;
- globally optimal expert grouping or placement;
- completed WAN/NAT/relay operation;
- automatic onboarding of arbitrary participant hardware;
- that all expert traffic should be distributed remotely when useful capacity is available locally.

Local expert co-location remains a valid placement option, and stateful Block Nodes remain a separate way to reduce repeated network boundaries.

## Design conclusion

The accepted result is that **physically distributed Expert Groups are compatible with Pluruno's strict pretrained-model execution semantics** in the current controlled proof environment.

This strengthens the project's multi-role design: model capacity can be divided into deployment units that fit heterogeneous peers while preserving exact logical expert identity.