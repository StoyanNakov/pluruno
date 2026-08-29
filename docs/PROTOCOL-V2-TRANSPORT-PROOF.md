# Protocol v2 Transport Proof

Pluruno's early distributed-MoE experiments exposed a basic transport problem: sending one external RPC per individual expert makes the inference path far too chatty. In the original path, even a tiny generation could require roughly 1,475 RPCs.

The accepted Protocol v2 direction changes the transport granularity rather than changing the pretrained model semantics.

## Proven transport shape

For each MoE layer, the Session / Model Executor keeps the pretrained gate authoritative, determines the exact selected experts, groups the selected unique experts by destination worker, and sends one grouped `layer_batch` request per participating worker over a persistent connection.

The accepted transport properties are:

- persistent connections rather than reconnecting for every expert call;
- grouped per-layer expert requests by destination worker;
- one external inference endpoint per worker, with backend isolation behind it;
- FP16 as the default wire representation;
- exact logical expert identity preserved;
- same-request retry may use an identical replica, without silently substituting another logical expert;
- activation payload is retained until the request has a final outcome;
- telemetry is kept out of the synchronous inference hot path.

This is a transport/data-plane optimization. It does not change the native top-k routing decision of the pretrained model.

## Frozen correctness/performance control

The accepted FP16 control for this transport path produced:

- generation mean: **8.320 s**
- TTFT: **4.981 s**
- decode: **0.556 s/token**
- throughput: **1.797 tok/s**
- fallback count: **0**
- telemetry errors: **0**
- exact expected output: **PASS 3/3**

These numbers are a controlled reference point for the tested topology, not a claim of general performance across hardware or networks.

## A rejected batching idea

A separate experiment added a **1 ms transport microbatcher** above the router-side transport path. It worked mechanically, but performance became worse. That approach was rejected and is not part of the accepted transport design.

A model-level static batch experiment also confirmed that same-expert activations can naturally merge across sequences, but its measured performance was not good enough to treat static batching as the production solution.

The result is an important distinction:

> reducing RPC count is useful, but adding batching latency or assuming that static model batching is always beneficial is not.

## Decomposition evidence

A controlled decomposition of the earlier topology measured:

### Batch 1

- wall time: **8.086 s**
- executor/router-local work: **4.344 s**
- transport component: **3.742 s**
- remote worker compute: **1.003 s**

### Batch 4

- wall time: **25.564 s**
- executor/router-local work: **13.892 s**
- transport component: **11.672 s**
- remote worker compute: **1.351 s**

The measurements show why Pluruno does not treat network bandwidth as the only distributed-inference cost. Executor-side work, transport behavior and remote compute need to be measured separately.

## What this proof does and does not establish

It establishes that the tested Pluruno path can preserve exact pretrained MoE routing semantics while reducing external transport chatter through persistent, grouped per-layer requests.

It does **not** establish that this transport is optimal for every topology. Internet links, high-RTT paths, different accelerators, different batch shapes and future block/pipeline deployments still require separate measurement.

The broader design rule remains: optimize the data plane from measured topology behavior, while keeping model semantics and exact logical expert identity explicit.