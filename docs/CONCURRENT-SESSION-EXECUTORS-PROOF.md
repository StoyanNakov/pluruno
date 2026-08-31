# Concurrent Session Executors Proof

Pluruno separates global coordination from session-specific model execution. This document records the accepted controlled-lab result that multiple independent Session / Model Executors can operate under one control-plane architecture.

## Question

Can Pluruno run more than one independent Session Executor at the same time without requiring the Global Control Plane itself to become the model-execution hot path?

## Accepted result

The controlled Linux lab demonstrated two concurrent execution domains under the same overall control-plane architecture.

Observed generation throughput in the accepted runs was:

| Execution domain | Observed throughput |
|---|---:|
| Domain A | 5.643 tok/s |
| Domain B | 8.263 tok/s |

These values are reported as separate observations. They should not be interpreted as a universal scaling ratio, a claim of linear speedup, or a benchmark that can be summed across arbitrary workloads and hardware.

## Architectural meaning

Each Session Executor owns its session-specific hot path, including non-expert model computation, attention and KV state, native MoE routing decisions, activation packing, remote expert orchestration, expert result combination, and the generation loop.

The Global Control Plane coordinates system-level state and placement, but does not need to execute every activation for every active session.

Conceptually:

```text
                 Global Control Plane
                  /               \
                 /                 \
        Session Executor A   Session Executor B
          execution domain     execution domain
```

The accepted result therefore supports a control-plane/data-plane separation in which multiple execution domains can coexist instead of forcing all inference through one centralized Session Executor.

## Why this matters

A heterogeneous distributed inference system should be able to place sessions near suitable compute and model capacity. Independent Session Executors make it possible to treat executor placement as a per-session or per-domain decision rather than a permanent property of the control-plane machine.

This also avoids turning the Global Control Plane into an unavoidable sequential latency point for every model layer and token.

## Relationship to the placement proof

`SESSION-EXECUTOR-PLACEMENT-PROOF.md` showed that moving the executor hot path away from a weak CPU materially improved one controlled distributed MoE workload while the remote expert path remained unchanged.

This document records a different accepted result: **more than one independent Session Executor can operate concurrently under the same overall control-plane architecture.**

## Scope

This is a controlled laboratory proof from the current Pluruno research program. It does not claim linear multi-session scaling, arbitrary workload isolation, Internet-scale concurrency, or production multi-tenant quality-of-service guarantees.

The supported conclusion is deliberately narrower: **Pluruno has demonstrated concurrent independent Session Executor domains without requiring the Global Control Plane to become the inference hot path.**
