# Session Executor Placement Proof

Pluruno separates the global control plane from the latency-sensitive Session / Model Executor role. This document records the controlled experiment that established that design choice.

## Question

Does moving the Session / Model Executor hot path away from a weak CPU materially improve distributed MoE inference when the remote Expert Group and its constrained network path stay unchanged?

## Controlled comparison

The remote Expert Group remained on the same intentionally constrained approximately 100 Mb/s path in both runs. The main experimental change was the placement of the Session / Model Executor hot path.

| Metric | Weak executor CPU | Stronger executor CPU | Change |
|---|---:|---:|---:|
| Generation | 8.320 s | 4.751 s | -42.9% |
| TTFT | 4.981 s | 2.976 s | -40.25% |
| Decode | 0.556 s/token | 0.296 s/token | -46.8% |
| Throughput | 1.797 tok/s | 3.379 tok/s | +88.0% |

Correctness remained stable in the accepted runs, with exact expected output in 3/3 repetitions and no fallback jobs.

## Control against hidden GPU acceleration

The stronger executor's GPU memory usage remained at 209 MiB before and after the experiment. The measured improvement therefore was not explained by silently moving model compute onto that GPU.

The dominant change was the Session / Model Executor CPU/hot-path placement.

## What this proved

The experiment showed that an apparently network-limited distributed topology can still be dominated by executor-side compute and orchestration overhead.

That means nominal link bandwidth alone is not a sufficient predictor of end-to-end distributed inference cost. Pluruno's placement model therefore treats the following as separate concerns:

- global control-plane placement;
- Session / Model Executor capability;
- remote expert placement;
- network capability and topology;
- current workload and queue state.

The architectural result is:

```text
Global Control Plane != Session / Model Executor
```

The control plane may coordinate placement and health without being forced into every latency-sensitive inference activation path.

## Scope

This is a controlled laboratory proof from the current Pluruno research program. It is not a claim that the same percentages generalize to every model, CPU, GPU, network or workload.

The useful general result is narrower: **executor placement can materially change distributed inference performance even when the remote expert topology and constrained network path are held constant.**
