# Scheduler Observability Proof

## Scope

This note records a narrow capability already demonstrated by the current Linux prototype: placement decisions can be made from measured capability and objective signals, and the resulting scheduler reasoning can be exposed through operator-facing observability.

## Proven boundary

The accepted Linux work demonstrated:

- capability-aware role promotion;
- selected and rejected candidate reasoning;
- objective-aware scheduler decisions;
- visibility of those decisions through dashboard/API observability.

This means placement is not required to be an opaque machine-selection step. Operators can inspect why an eligible candidate was selected or rejected and relate that decision to the scheduler objective being exercised.

## Architectural consequence

Observability is part of the scheduling contract, not an afterthought. A placement decision should preserve enough non-sensitive reasoning to support validation, comparison, and diagnosis without putting the global control plane into every inference activation path.

The model's native routing remains authoritative for logical expert selection. Scheduler observability explains physical placement decisions; it does not authorize substitution of a different logical expert.

## Claim limits

This proof does **not** claim:

- globally optimal scheduling;
- universal hardware or operating-system coverage;
- validated WAN/Internet placement;
- autonomous remediation of every placement failure;
- that every future scheduler objective is already implemented.

It records only the accepted capability demonstrated in the current Linux experimental program.

## Status

**PROVEN within the accepted Linux prototype boundary.**
