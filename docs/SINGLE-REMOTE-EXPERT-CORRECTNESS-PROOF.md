# Single Remote Expert Correctness Proof

Pluruno's distributed MoE work began with a deliberately small correctness test before attempting full-model distributed execution.

The question was simple:

> Can one model-selected expert execute on a remote physical peer and return the same numerical result as the corresponding local expert execution?

## Proof setup

The controlled experiment used the pretrained model:

`allenai/OLMoE-1B-7B-0125-Instruct`

The model's native router/gate remained responsible for selecting the logical expert. Pluruno changed only the physical location where that exact expert computation was executed.

```text
model router/gate
      |
      | selects logical expert E
      v
Pluruno placement
      |
      +--> local exact E
      |
      +--> remote exact E
```

The remote path was not allowed to substitute a different logical expert.

## Correctness check

For the proof, the same selected expert computation was evaluated through both paths:

1. execute the selected expert locally;
2. execute that exact expert remotely on another physical peer;
3. return the remote result;
4. compare the local and remote outputs numerically.

The observed numerical difference in the accepted proof was below `1e-6`.

This established the first correctness boundary required by the larger distributed system: moving an exact expert computation across the network did not materially change its output in the controlled test.

## Why this mattered

A distributed MoE system is not correct merely because a remote GPU can execute some expert-shaped computation. The pretrained model's routing semantics must remain intact.

Pluruno therefore separates two decisions:

- **logical routing:** the pretrained model selects the expert;
- **physical placement:** Pluruno selects where an exact deployment of that expert executes.

The single-expert comparison validated this separation before progressing to multi-peer execution, the complete 16-layer distributed forward path, autoregressive generation, and exact-replica failover.

## What this proves

The accepted experiment demonstrates that, for the tested model and controlled Linux environment, an individually selected MoE expert can be executed on a remote physical peer with numerical output matching the corresponding local execution to within the observed `<1e-6` difference.

## What this does not prove

This narrow proof does not establish:

- correctness for every MoE architecture or numerical format;
- public-Internet readiness;
- arbitrary untrusted-peer execution;
- production-grade fault tolerance;
- performance superiority of remote execution;
- a universal participant package.

Those are separate engineering and validation questions.

## Publication boundary

This note records the accepted experimental result and its semantics. Machine-specific addresses, credentials, private runtime state, internal deployment identifiers, and raw operational evidence are intentionally excluded.