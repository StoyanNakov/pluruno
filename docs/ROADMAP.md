# Pluruno Roadmap

This roadmap separates **verified work**, **next engineering phases**, and **longer-term research direction**. Items listed here are not production claims unless they are explicitly marked as verified elsewhere.

## Completed experimental foundations

The Linux research prototype has already validated:

- distributed exact MoE expert execution;
- Global Control Plane separation from Session / Model Executors;
- two simultaneous execution domains;
- physically distributed Expert Groups;
- local expert co-location;
- exact replica retry;
- Session Executor failure isolation;
- single and packed Expert Nodes;
- capability-aware role selection;
- stateful Block Nodes;
- Block Node sizing experiments;
- scheduler decision explainability and dashboard visibility.

See [Current Status](CURRENT-STATUS.md) for the verified state.

## T4 — Windows + Linux mixed swarm

**Next major phase.**

Goals:

- discover and freeze Windows peer hardware/runtime capability;
- evaluate native Windows vs WSL2 vs container-based participation;
- establish persistent identity and management transport;
- qualify NVIDIA GPU capability;
- retrieve artifacts/models safely;
- assign roles from measured capability;
- execute mixed-OS inference;
- verify heartbeat, failure, and recovery behavior;
- preserve full dashboard visibility.

The runtime choice will be made from measured evidence rather than assumed in advance.

## WAN validation

After mixed-OS participation is stable, validate the architecture under controlled WAN-like conditions.

Planned dimensions include:

- latency/RTT;
- bandwidth;
- jitter;
- packet loss;
- asymmetric links;
- stragglers;
- peer disappearance.

Measure separately:

- prefill latency;
- decode latency;
- TTFT;
- time/token;
- aggregate throughput;
- bytes/token;
- topology hop count;
- exact correctness;
- fallback/retry behavior;
- executor and worker utilization.

## Locality and regional execution

Research and implement session/regional execution cells so hot inference traffic stays near the active executor rather than repeatedly crossing a global network path.

Areas include:

- executor-to-peer RTT-aware placement;
- measured bandwidth-aware placement;
- local expert co-location;
- exact replicas by region/cell;
- expert affinity;
- fewer sequential network synchronization points;
- direct executor-to-peer data paths.

## Multi-model manifests

Move from a proof-model-specific implementation toward manifest-driven execution.

A model manifest should eventually describe:

- model ID;
- immutable revision/hash;
- architecture;
- layer/expert topology;
- top-k behavior;
- tensor/deployment sizes;
- supported codecs;
- allowed deployment-unit types;
- executor requirements;
- exactness requirements;
- content hashes and compatibility constraints.

Target model families may include additional pretrained MoE architectures after the execution contract becomes sufficiently generic.

## Resource residency

Introduce explicit deployment residency states:

- HOT — accelerator memory;
- WARM — system memory;
- COLD — local storage.

Scheduling should react to demand without causing constant model-weight churn.

## Public-peer security

Before arbitrary Internet peers are accepted, the project needs a dedicated security phase covering at least:

- authenticated worker identity;
- encrypted transport;
- signed manifests;
- immutable model revisions;
- replay protection where required;
- rate limiting;
- malicious-worker detection;
- sandbox/container isolation;
- prompt and privacy policy;
- secure update/artifact distribution.

See [SECURITY.md](../SECURITY.md).

## Contribution accounting and reputation

Longer-term research may measure useful contribution using signals such as:

- actual successful computation;
- uptime and reliability;
- latency and bandwidth;
- throughput;
- VRAM/capacity;
- rare model/deployment hosting;
- geographic coverage;
- redundancy provided.

The accounting model should reward **useful verified work**, not merely hardware ownership.

## Credits / network economy

A contribution-credit system is part of the project vision but is not the current engineering priority.

Potential goals include:

- contributors earn credit for useful verified service;
- credit can be spent on network inference;
- users without hardware may eventually obtain usage through a separate mechanism;
- economics should not weaken correctness or security guarantees.

No blockchain dependency is assumed. Any blockchain/token design would be an optional later decision, not a prerequisite for distributed inference.

## Open research questions

Some major questions deliberately remain open:

- Which workloads benefit most from Expert Nodes vs Expert Groups vs Block Nodes?
- How should scheduler objectives trade TTFT, decode throughput, bandwidth, locality, and reliability?
- How much expert affinity can reduce WAN traffic for frozen pretrained models?
- When does local co-location beat stronger but distant hardware?
- How should the network validate useful work from untrusted peers?
- How should heterogeneous AMD/NVIDIA/other accelerators coexist behind a common capability model?
- What is the best deployment boundary for Windows contributors?

These questions are expected to be answered experimentally rather than by fixed assumptions.
