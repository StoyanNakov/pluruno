# Full 16-Layer Distributed Forward Proof

After validating a single remote expert numerically, Pluruno expanded the experiment to a complete forward pass through the proof model while executing selected MoE experts across multiple physical peers.

## Proof model

The controlled experiment used:

`allenai/OLMoE-1B-7B-0125-Instruct`

For this proof the model contains:

- 16 MoE layers;
- 64 logical experts per MoE layer;
- native top-k = 8;
- 1024 layer-expert instances across the model.

The pretrained router/gate remained authoritative for logical expert selection. Pluruno controlled only the physical placement of the exact selected expert deployments.

## Progression to the full forward path

The experiment deliberately progressed in stages:

1. validate one selected expert through local and remote execution;
2. preserve the model-native logical expert identity;
3. distribute exact expert deployments across multiple physical peers;
4. execute the model through every one of its 16 MoE layers using the distributed expert path;
5. confirm that the complete forward pass finishes successfully.

The accepted result was a successful **full 16-layer distributed forward pass**.

This is a stronger system-level result than the single-expert comparison. It demonstrates that remote expert execution was not limited to an isolated RPC or one layer: the distributed path remained functional across the complete sequence of MoE layers in the tested model.

## Correctness boundary

The execution rule remained strict throughout the proof:

```text
native model router/gate
        |
        | chooses logical expert
        v
Pluruno placement
        |
        | chooses an exact physical deployment
        v
local or remote execution
```

Physical distribution did not authorize Pluruno to replace the logical expert chosen by the pretrained model.

## What this proves

For the tested model and controlled Linux environment, Pluruno demonstrated that a complete 16-layer MoE forward path can execute while exact selected experts are physically distributed across networked peers.

Together with the earlier single-expert numerical comparison, this established the foundation for the later autoregressive generation, exact-replica failover, grouped transport, placement, and scheduling experiments.

## What this does not prove

This proof does **not** establish:

- production readiness on the public Internet;
- correctness for every MoE model architecture;
- acceptable performance on every latency or bandwidth regime;
- arbitrary untrusted-peer execution;
- universal automatic participant onboarding;
- a completed multi-model serving system.

Those remain separate engineering and validation problems.

## Publication boundary

This public proof records the accepted execution semantics and result without exposing machine-specific addresses, credentials, private runtime state, raw internal logs, internal identifiers, or deployment-specific operational details.