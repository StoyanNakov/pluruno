# Session Executor Failure Isolation Proof

Pluruno separates global coordination from session-specific model execution. This note records the accepted controlled-lab result that a Session / Model Executor is an independently placed execution role whose failure is isolated from the Global Control Plane role.

## Question

Can the session-specific inference hot path fail as an execution-domain event without turning that failure into a failure of the global coordination role itself?

## Accepted result

The Pluruno Linux research prototype has validated **Session Executor failure isolation** as part of its completed experimental foundations.

The Session Executor owns session-specific model execution state and orchestration, while the Global Control Plane owns system-level coordination and desired state. Because these responsibilities are separated, loss of a Session Executor is treated as a failure of that execution domain rather than evidence that the Global Control Plane must itself execute the model hot path.

This result is complementary to the concurrent Session Executor proof: multiple execution domains can coexist under one control-plane architecture, and the execution role is not permanently fused to the global coordinator.

## Architectural meaning

Conceptually:

```text
                 Global Control Plane
                   /            \
                  /              \
        Session Executor A   Session Executor B
          execution domain     execution domain
```

A Session Executor owns its inference-domain responsibilities, including session-specific model computation, attention/KV state, MoE routing orchestration, remote execution coordination, result combination, and generation flow.

The Global Control Plane remains responsible for coordination and placement state, but is not required to become the synchronous inference executor for every session.

Failure isolation therefore means that the Session Executor lifecycle is an execution-domain concern rather than a requirement to collapse control-plane and data-plane responsibilities back into one process or machine.

## Relationship to other proofs

This result is distinct from exact-replica failover.

- **Exact-replica failover** concerns recovery of a requested logical expert through another exact physical replica.
- **Session Executor failure isolation** concerns separation of the session/model execution role from the global coordination role.
- **Concurrent Session Executors** demonstrates that more than one independent execution domain can operate under the same overall control-plane architecture.

Together, these results support a design in which coordination, session execution, and distributed expert execution have explicit failure boundaries.

## What is not claimed

This proof does **not** claim:

- automatic live migration of an active Session Executor;
- zero-loss recovery of arbitrary in-flight generation state;
- active-active highly available control-plane operation;
- production multi-tenant fault containment guarantees;
- production WAN or public-Internet failover;
- universal recovery behavior across arbitrary models and hardware.

Those remain separate engineering stages.

## Supported conclusion

The supported conclusion is deliberately narrow: **Pluruno has experimentally validated Session Executor failure isolation as a consequence of keeping session-specific inference execution separate from the Global Control Plane role.**

## Publication boundary

This note intentionally omits machine-specific addresses, hostnames, credentials, private runtime state, local filesystem paths, raw operational evidence, and deployment-sensitive security details.
