# Security

Pluruno is currently an **experimental research prototype** and is **not ready for arbitrary untrusted Internet peers**.

## Current security boundary

The current research implementation is developed and tested in a controlled environment. Public documentation intentionally omits private lab addresses, credentials, host-specific paths, service identifiers, and deployment-sensitive operational details.

Do not expose a current experimental node directly to the public Internet based only on the documentation in this repository.

## Before public-peer deployment

A dedicated hardening phase is required before Pluruno can safely accept untrusted community peers. Planned areas include:

- authenticated worker identity;
- encrypted transport (for example TLS/QUIC-TLS or equivalent);
- signed deployment/model manifests;
- immutable model revision and content identity;
- replay protections where applicable;
- rate limiting and abuse controls;
- malicious-worker detection and result validation;
- sandbox/container isolation;
- secure artifact/update distribution;
- prompt/data privacy policy;
- reputation and trust signals.

These are roadmap requirements, not claims of current implementation.

## Reporting a vulnerability

Please do **not** publish sensitive exploit details, credentials, private infrastructure information, or a working attack against a real deployment in a public issue.

Until a dedicated private security-reporting channel is configured, open a minimal public issue stating that you have a security concern **without including exploit details or secrets**, and request a private contact path.

## Scope reminder

Because Pluruno is experimental, protocol and deployment interfaces may change. Security designs should assume heterogeneous, intermittently connected, and eventually untrusted peers rather than relying on a permanently trusted homogeneous cluster.
