# FP16 Transport Baseline Measurement

Pluruno uses FP16 as the current default activation transport format in its experimental distributed inference path. This note records the accepted controlled measurement behind that baseline.

## Why establish a baseline

Reducing activation payload size can be attractive on constrained links, but a smaller wire format is useful only if the complete inference path benefits without silently changing the model's execution semantics.

For that reason, Pluruno treats the uncompressed FP16 transport path as the reference point against which optional lossy transport experiments are compared.

## Accepted measurement

In the controlled Protocol v2 transport experiment, the FP16 path measured approximately:

`1.904 tok/s`

A Q8_0 transport experiment in the same experimental line measured approximately:

`1.777 tok/s`

The measured Q8_0 run therefore did not outperform the FP16 baseline. FP16 remains the current default transport format, while lossy activation transport remains explicit opt-in research rather than a default assumption.

## What the result means

The result is intentionally narrow. It demonstrates that, in the measured Pluruno lab configuration, reducing the activation representation to Q8_0 did not translate into higher end-to-end generation throughput.

This reinforces an important scheduling and transport principle:

> Wire size alone is not an adequate optimization target; serialization, conversion, execution, synchronization, network characteristics, and end-to-end token latency must be measured together.

This is particularly relevant to heterogeneous execution, where Pluruno has tested materially different network links rather than assuming a uniform high-speed fabric.

## Correctness boundary

Transport optimization does not change Pluruno's strict routing rule. The pretrained model's native router/gate remains authoritative about which logical expert is required. Physical placement and transport choices must not silently substitute another logical expert.

## What this does not prove

This measurement does **not** establish that:

- FP16 is optimal for every network or model;
- Q8_0 is always slower than FP16;
- the measured throughput generalizes to public Internet deployment;
- lossy activation transport is production-ready;
- the current experimental system is a production service.

Different bandwidth, latency, hardware, batching, payload sizes, and model architectures may change the trade-off and require new measurements.

## Publication boundary

This public note records only the accepted experimental result and its engineering interpretation. It intentionally excludes private machine addresses, hostnames, credentials, internal deployment identifiers, filesystem paths, databases, and raw operational evidence.