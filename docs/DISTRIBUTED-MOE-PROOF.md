# Distributed MoE Proof

Pluruno began with a narrow experimental question:

> Can a pretrained Mixture-of-Experts model keep its native routing decision while executing the selected experts on different physical machines over the network?

The controlled Linux proof established that this is possible for the current experimental model.

## Proof model

The primary proof model was:

`allenai/OLMoE-1B-7B-0125-Instruct`

For this experiment the model has:

- 16 MoE layers;
- 64 experts per MoE layer;
- native top-k = 8;
- hidden size 2048;
- 1024 layer-expert instances across the model.

The model was selected because it is small enough for a heterogeneous lab while still exercising a real pretrained MoE routing path.

## Execution rule

The correctness boundary is important:

```text
pretrained model router/gate
        decides
which logical expert is required

Pluruno
        decides
which exact physical deployment executes it
```

Pluruno does not change the model-native logical expert selection in strict execution mode.

## What was demonstrated

The initial distributed proof progressed from a single remote expert to full model execution:

1. A selected expert executed remotely on another physical machine.
2. The returned expert output was compared with the corresponding local execution.
3. Single-expert numerical difference was observed below `1e-6` in the proof.
4. Expert execution was distributed across multiple physical peers.
5. A complete 16-layer forward pass succeeded using remote experts.
6. Autoregressive generation succeeded with distributed expert execution.

A simple end-to-end generation sanity check produced:

```text
2 + 2 equals 4.
```

The text itself is not the important result. The important result is that generation completed through the distributed expert path rather than requiring all experts to be local to one accelerator.

## What this proves

This experiment supports the core Pluruno thesis:

> Pretrained MoE expert computation can be physically distributed across networked peers while preserving the model's logical routing decision.

It also established the base needed for later Pluruno work on exact replicas, failover, multiple execution roles, heterogeneous scheduling, and topology-aware placement.

## What this does not prove

This result is a controlled-lab engineering proof. It does **not** by itself establish:

- production readiness on the public Internet;
- safe execution with arbitrary untrusted peers;
- acceptable performance on every network topology;
- universal support for arbitrary MoE architectures;
- a completed public participant package.

Those are separate validation problems.

## Reproducibility boundary

This public note intentionally documents the experimental semantics and accepted result without publishing machine-specific addresses, credentials, private runtime state, raw operational logs, or internal deployment details.

Future public proof notes can add sanitized benchmarks and reusable test methodology as those artifacts are reviewed for publication.