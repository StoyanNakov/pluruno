# Q8_0 Wire-Format Experiment

Pluruno tested whether a lossy `Q8_0` activation wire representation could improve distributed inference performance compared with the current FP16 transport path.

This was a controlled experiment on the existing distributed-MoE proof topology. It was not a production WAN benchmark and it does not establish a general result for every model, accelerator, or network.

## Measured result

The accepted comparison recorded approximately:

| Wire representation | Throughput |
|---|---:|
| FP16 | **1.904 tok/s** |
| Q8_0 | **1.777 tok/s** |

In this measured run, `Q8_0` did **not** outperform FP16. The observed throughput was lower, so FP16 remains the default Pluruno wire representation and lossy transport remains explicit opt-in research.

## Why this matters

A smaller payload does not automatically produce faster end-to-end inference.

Distributed inference cost includes more than transferred bytes. Encoding and decoding work, executor-side processing, request granularity, synchronization, remote compute, topology, and network behavior can all dominate or offset the bandwidth saving from quantization.

The result therefore supports a measurement-first rule:

> Do not select a lossy wire format from payload size alone. Measure end-to-end latency and throughput on the target topology while preserving correctness boundaries.

## Design consequence

Pluruno keeps transport representation separate from logical model routing semantics:

- the pretrained model router remains authoritative about the logical expert selection;
- Pluruno may experiment with how activations are represented in transit;
- lossy transport must be explicit rather than silently changing the default execution path;
- a codec is accepted only when its measured end-to-end tradeoff is justified for the target topology.

This also means a future topology-aware scheduler may choose different transport policies only after those policies have their own validated compatibility and performance evidence.

## What this experiment does not prove

This result does **not** prove that Q8_0 is always slower than FP16.

It does not establish results for:

- arbitrary MoE or dense models;
- public-Internet links;
- higher-latency WAN paths;
- other quantization formats;
- different CPU/GPU codec implementations;
- every activation shape or batch size.

A lower-bandwidth or higher-latency path could produce a different crossover point and requires a separate experiment.

## Publication boundary

This note reports only the accepted experimental outcome and design consequence. It intentionally excludes machine-specific addresses, credentials, private runtime state, raw internal logs, and operational deployment details.
