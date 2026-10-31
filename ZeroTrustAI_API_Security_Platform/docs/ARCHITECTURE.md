# Architecture

## Services and boundaries

| Component | Responsibility | Not responsible for |
| --- | --- | --- |
| Gateway | Verify API identity, derive features, evaluate policy, forward to a fixed origin | Knowing which user owns a backend order |
| Demo API | Enforce object ownership and role rules on sample data | Public gateway management |
| Redis | Shared rolling counters, status history, expiring quarantine | Durable event/audit history |
| PostgreSQL | Persist redacted security events in Docker | Immutable/tamper-evident audit guarantees |
| Dashboard | Display admin-authorized summaries, events, and controls | Authorizing API access in the browser |
| Offline trainer | Fit/calibrate a trusted Isolation Forest artifact | Automatically learning from unreviewed live traffic |

Docker publishes only the gateway, on loopback port 8000. The backend and data
services are reachable on the Compose network but are not host-published.
Local non-Docker development uses SQLite and a single-process memory state option.

## Request lifecycle

1. Validate the proxy path, URL length, and bounded request body.
2. For protected routes, verify JWT signature, issuer, audience, expiry, and required claims.
   Demo login and health routes have explicitly defined public access.
3. Key authenticated history by verified subject; use the socket peer for unauthenticated
   traffic. Do not trust caller-provided forwarding headers as an identity.
4. Reject active quarantines. Atomically update a rolling 60-second request window.
5. Derive 18 numeric features using the current request and **past** status history.
6. Run optional Isolation Forest inference off the async event loop. Combine its bounded
   contribution with deterministic behavioral rules.
7. Allow, return `429`, or block/quarantine. No fake MFA challenge is issued.
8. Forward allowed requests to the configured backend origin without following redirects.
9. The backend independently enforces ownership/roles. Its `401`/`403` is logged as an
   explicit authentication/authorization denial, not an ML detection success.
10. Persist a redacted event and record response status for future request context.

Body/response buffering, inspection timings, and dependency failures have deliberate
limitations documented in the security notes. Event persistence is not a distributed
transaction with upstream side effects.

## Management flow

Dashboard login issues a JWT with audience `zerotrust-admin`. Demo identities use
`demo-api`; these tokens are not interchangeable. All event/quarantine endpoints require
the administrator audience and role. Tokens stay in browser memory and expire after
15 minutes. An authenticated fetch stream reads SSE summaries; five-second polling
refreshes events and provides a fallback.

## Why these choices?

- **FastAPI + HTTPX:** keep the request flow readable and testable as a Python academic
  prototype; not a claim of Envoy-equivalent proxy behavior.
- **Isolation Forest first:** a small reproducible unsupervised baseline before adding
  a neural model. Synthetic calibration cannot establish real attack detection.
- **Explicit authorization:** anomaly detection cannot establish resource ownership.
- **Local frontend assets:** no Node.js build or third-party scripts required to run.
- **Docker Compose:** demonstrate a separate backend boundary without paid cloud services.

## Evaluation boundaries

Functional tests establish specific control behavior, not security certification.
Compare rules, model-only, and combined results on independently labeled data before
claiming detection improvements. Measure direct versus proxied workloads under documented
hardware and concurrency before claiming latency overhead or throughput.
