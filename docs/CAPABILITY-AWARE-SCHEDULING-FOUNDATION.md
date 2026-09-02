# Capability-Aware Scheduling Foundation

Pluruno has experimentally validated a **capability-aware scheduling foundation** for heterogeneous distributed AI execution.

This result is intentionally narrower than a claim of a finished production scheduler. It demonstrates that role selection can be driven by observed machine and topology characteristics rather than assuming identical peers or assigning one permanent role to each machine.

## What was demonstrated

The controlled Linux research environment has already demonstrated:

- heterogeneous peers with materially different CPU, GPU, memory, and network characteristics;
- capability-aware role selection;
- placement of different execution roles on different physical machines;
- Session Executor placement that changes measured inference performance;
- local model-capacity co-location as a valid placement option;
- scheduler decision visibility, including why candidates are selected or rejected;
- operation across links with materially different bandwidth classes, including approximately 100 Mb/s and approximately 1 Gb/s paths.

These results support a scheduler model in which placement is based on measured capability, topology, locality, and workload rather than a homogeneous-cluster assumption.

## Why this matters

Distributed inference performance depends on more than accelerator speed. A stronger GPU can be a worse placement target when the network path, CPU-side work, queueing, or locality costs dominate.

Pluruno therefore treats placement as a multi-factor decision. Relevant inputs can include:

- model/deployment compatibility;
- executor-to-peer latency;
- measured bandwidth;
- GPU and CPU capability;
- available accelerator and system memory;
- current load;
- topology and locality;
- replica availability;
- deployment residency state.

The scheduler is also intended to remain inspectable: an operator should be able to see the selected candidate and the reasons alternatives were not selected.

## Established boundary

This proof establishes the **foundation**, not a finished universal scheduler.

It does **not** claim that Pluruno has completed:

- automatic public participant onboarding;
- universal measured qualification for arbitrary hardware;
- production WAN or Internet-wide placement;
- production multi-model residency management;
- production reputation or contribution scoring;
- globally optimal placement under every workload.

Those remain separate engineering and research stages.

## Design conclusion

The accepted result is that heterogeneous role placement and scheduler explainability are viable parts of the Pluruno architecture. Machines do not need to be identical, and placement decisions can be made from measured characteristics while keeping the reasoning visible to the operator.

This supports the project's broader principle:

> Adapt the role to the machine, not the machine to the role.
