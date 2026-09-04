# Expert Node Packing Proof

Pluruno supports several deployment granularities for Mixture-of-Experts execution. This note records the accepted controlled-lab result for the smallest of those roles: the Expert Node.

## Question

Can the same execution role represent either a single exact expert or a small packed set of exact experts without changing the pretrained model's logical routing decision?

## Accepted result

The Pluruno Linux research prototype has validated both:

- a single-expert Expert Node; and
- a packed Expert Node containing a small number of exact experts.

In both cases, the deployment unit remains subordinate to the pretrained model's native MoE routing decision. The model chooses the logical expert; Pluruno chooses an eligible physical deployment that contains that exact expert.

Conceptually:

```text
native model gate
      |
      v
required logical expert
      |
      +--> single-expert Expert Node
      |
      +--> packed Expert Node containing that exact expert
```

Packing therefore changes physical placement granularity, not model semantics.

## Why this matters

A heterogeneous network should not require every participant to host the same amount of model state.

The Expert Node role provides a fine-grained placement option for peers that can host one or a small number of experts, while packing allows several exact experts to share one execution location when that is operationally useful.

This complements broader Expert Group and Block Node roles rather than replacing them. Pluruno can choose deployment granularity according to measured capability, locality, available memory, and workload while preserving exact logical expert identity.

## Correctness boundary

The accepted result does not permit substituting a different logical expert merely because it is colocated on the same peer.

In strict pretrained-model execution:

```text
logical expert identity is fixed by the model
physical placement is selected by Pluruno
```

A packed node is therefore a container for multiple independently addressable exact experts, not a license to alter routing semantics.

## Scope

This is a controlled-laboratory proof for the current Pluruno research prototype. It does not claim:

- that one packing size is universally optimal;
- production public-Internet performance;
- arbitrary-model compatibility;
- automatic participant onboarding;
- universal heterogeneous-hardware qualification; or
- production multi-tenant scheduling guarantees.

The supported conclusion is deliberately narrow: **Pluruno has demonstrated both single and packed Expert Node deployment while preserving exact logical expert selection semantics.**

## Publication boundary

This note intentionally omits machine-specific addresses, hostnames, credentials, private runtime state, local filesystem paths, raw operational evidence, and deployment-sensitive security details.
