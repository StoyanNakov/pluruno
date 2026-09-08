# Router Quantization Finding

Pluruno tested an important correctness question for distributed Mixture-of-Experts inference:

> Can router/gate quantization be treated as a harmless implementation detail when exact pretrained-model routing semantics matter?

The accepted experiment found that the answer is **no** for the tested quantization path. Quantizing the router changed a measurable fraction of expert-routing decisions, so Pluruno keeps the model-native router/gate outside lossy transport or placement optimizations unless a separate correctness case is established.

## Measured result

The comparison covered **6,736 routing positions**.

| Routing comparison | Observed change |
|---|---:|
| Ordered top-k changed | **24.26%** |
| Top-k membership changed | **7.76%** |
| Rank-only change | **16.49%** |
| Uncontaminated membership change | **5.50%** |

The distinction matters. An ordered top-k difference can be only a rank change among the same selected experts, while a membership change means that at least one different logical expert is selected.

Even the smaller membership-change rate is material when Pluruno's strict execution rule is to preserve the pretrained model's logical expert selection exactly and vary only the physical replica or placement used to execute that expert.

## Design consequence

Pluruno separates three decisions:

1. **The pretrained router/gate selects the logical expert set.**
2. **Pluruno selects the physical exact replica or deployment location.**
3. **Transport may optimize representation only where that optimization does not silently redefine logical routing semantics.**

This means lossy optimization of activations in transit is a different research question from lossy optimization of the router itself.

A wire codec can be benchmarked as a transport tradeoff. A router codec can change which computation the model performs, so it requires a stronger correctness evaluation before it can be considered equivalent.

## Why rank-only changes still matter

A rank-only change does not necessarily change top-k membership, but it should not automatically be ignored. Different MoE implementations may attach different weights or execution behavior to expert ordering, and downstream numerical effects depend on the model and kernel path.

For that reason Pluruno does not treat rank-only differences as proof of semantic equivalence. They are reported separately from membership changes rather than collapsed into one number.

## What this finding does not prove

This experiment does **not** establish that every router quantization method is unsafe or unusable.

It does not establish results for:

- every MoE architecture;
- every quantization format;
- quantization-aware trained routers;
- every precision level;
- every kernel implementation;
- downstream task-quality impact for all observed routing differences.

The finding is narrower: in the tested path, router quantization measurably changed routing decisions, so it cannot be assumed to preserve exact pretrained routing semantics.

## Current Pluruno rule

For strict execution, the model-native router/gate remains authoritative. Pluruno optimizes **where** an already-selected logical expert executes before considering any optimization that could change **which** logical expert the model selected.

This preserves a clean correctness boundary between model semantics and distributed placement.

## Publication boundary

This note reports the accepted aggregate result and resulting design rule. It intentionally excludes machine-specific addresses, credentials, private runtime state, raw operational logs, and internal deployment details.
