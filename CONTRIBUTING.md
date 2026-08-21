# Contributing to Pluruno

Pluruno is an experimental distributed AI systems project. Contributions are welcome, especially from people interested in heterogeneous hardware, distributed inference, networking, observability, scheduling, Windows/Linux interoperability, and correctness testing.

## Before contributing

Please keep these principles in mind:

1. **Correctness before speed.** A faster path is not accepted if it silently changes model semantics.
2. **Measured capability over assumptions.** Hardware and network roles should be chosen from evidence.
3. **Heterogeneous peers are expected.** A slower or smaller machine can still be useful in the right role.
4. **Control plane and hot data path are different concerns.** Avoid designs that force every activation through one global coordinator.
5. **Observability matters.** Distributed execution should remain inspectable.
6. **Do not expose secrets or private infrastructure.** Examples and evidence must be sanitized before publication.

## Good contribution areas

Current areas where outside help can be especially useful include:

- Windows / WSL2 / container runtime experiments;
- GPU capability discovery and qualification;
- portable worker packaging;
- transport/protocol work;
- latency and bandwidth measurement tooling;
- scheduler policy and explainability;
- Block Node execution and state handling;
- multi-model manifest design;
- failure injection and recovery tests;
- dashboard and observability improvements;
- documentation and reproducible benchmarks;
- AMD/other accelerator research;
- security design for future untrusted peers.

## Experimental discipline

When proposing a performance or topology change, include where possible:

- the exact hypothesis;
- what control/baseline is being compared;
- what changed;
- correctness criteria;
- performance criteria;
- rollback/recovery plan;
- evidence sufficient for another person to inspect the result.

A benchmark that merely runs is not necessarily a successful architecture result.

## Strict model semantics

In strict pretrained-model mode, the model's native gate decides the required logical expert. A contribution must not silently replace that logical expert with another one merely because a preferred physical peer is unavailable.

Replica selection and retry may choose another **identical physical replica** of the required logical deployment.

## Issues and pull requests

Before large changes, open an issue describing the problem and proposed direction so architecture-impacting work can be discussed first.

Small documentation fixes and clearly isolated improvements may go directly to a pull request.

When submitting a PR:

- keep the change focused;
- explain why it is needed;
- describe how it was tested;
- include relevant measurements for performance-sensitive changes;
- avoid committing generated model weights, credentials, tokens, private addresses, or machine-specific secrets.

## Project maturity

Interfaces and protocols may change while Pluruno moves through mixed-OS and WAN validation. Contributors should expect some experimental churn until the public-peer architecture becomes stable.

## License note

The repository license has not yet been finalized. Until a license is selected, please avoid submitting substantial code that depends on a particular licensing assumption. Documentation discussions, issues, design proposals, and small reviewable changes are still welcome.
