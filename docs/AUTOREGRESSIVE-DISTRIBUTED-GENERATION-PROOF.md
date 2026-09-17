# Autoregressive Distributed Generation Proof

After validating a single remote expert and a complete 16-layer distributed forward path, Pluruno tested whether the same distributed MoE execution path could sustain autoregressive token generation.

## Proof question

> Can a pretrained MoE model generate tokens autoregressively while model-selected experts execute across networked physical peers, without replacing the model's native logical routing decisions?

The controlled proof used:

`allenai/OLMoE-1B-7B-0125-Instruct`

For this model the tested path includes 16 MoE layers, 64 experts per layer, and native top-k routing of 8 experts.

## Execution semantics

At every generation step, the pretrained model remained authoritative for logical expert selection. Pluruno selected only the physical deployment used to execute each exact selected expert.

```text
previous tokens
     |
     v
model forward step
     |
     +--> native router selects logical experts
     |
     +--> exact expert execution may occur on remote peers
     |
     v
next token
     |
     +--------------------> repeat
```

A remote placement was not allowed to silently substitute a different logical expert.

## Accepted result

Autoregressive generation completed successfully through the distributed expert path.

A simple end-to-end sanity prompt produced:

```text
2 + 2 equals 4.
```

The wording of this output is not itself the proof. The relevant result is that multiple sequential generation steps completed while selected MoE expert computation was physically distributed rather than requiring every expert to reside on one accelerator.

## Why this is a separate milestone

A successful isolated expert call or a single full-model forward pass is necessary but not sufficient for usable generation. Autoregressive inference repeatedly exercises routing, remote execution, returned activations, and the next model step.

This proof therefore established that the distributed execution path could remain coherent across successive token-generation iterations in the controlled experiment.

It provided the base for later work on persistent transport, grouped expert requests, exact-replica retry, Session Executor placement, concurrent execution domains, and topology-aware placement.

## What this proves

For the tested pretrained model and controlled Linux environment, Pluruno successfully completed autoregressive generation while executing model-selected experts across networked physical peers and preserving native logical expert routing semantics.

## What this does not prove

This experiment does not establish:

- production readiness on the public Internet;
- universal support for arbitrary model architectures;
- optimal throughput or latency on every topology;
- safe execution with arbitrary untrusted peers;
- completed automatic participant onboarding;
- a universal public participant package.

Those remain separate engineering and qualification problems.

## Publication boundary

This public proof records the accepted execution semantics and result. It intentionally excludes machine-specific addresses, hostnames, credentials, internal identifiers, private runtime state, and raw operational logs.