# ZeroTrustAI roadmap

Unchecked items are planned work, not current capabilities.

## Foundation

- [x] Fixed-upstream REST gateway and protected demo API.
- [x] Separate administrator and demo JWT audiences.
- [x] Ownership and role enforcement in the backend.
- [x] Atomic Redis limits, temporary quarantine, audited releases.
- [x] PostgreSQL events, live dashboard, reproducible local scenarios.
- [x] Synthetic-data Isolation Forest training, calibration, and evaluation artifact.
- [x] Functional tests and CI configuration.

## Representative evaluation

- [ ] Collect sanitized benign sessions with realistic endpoint/method diversity.
- [ ] Define independent abuse labels and keep entire sessions out of multiple splits.
- [ ] Evaluate false positives, precision/recall, and per-scenario results.
- [ ] Record rules-only, model-only, and combined comparisons.
- [ ] Benchmark direct/proxied p50/p95/p99, errors, and throughput on documented hardware.

## Advanced implementation

- [ ] Add and evaluate the proposed 18→12→6→12→18 autoencoder.
- [ ] Compare a calibrated ensemble against the simpler baseline.
- [ ] Implement real TOTP enrollment, replay prevention, and route-bound step-up grants.
- [ ] Add durable management-login audits and policy/version history.
- [ ] Add schema migrations, event-export boundaries, and resource monitoring.

## Academic submission

- [ ] Verify every literature citation independently.
- [ ] Separate proposed targets from measured results in slides and report.
- [ ] Document failed experiments and limitations alongside successful demonstrations.
- [ ] Prepare an isolated, repeatable demo and explain each decision without the UI.

Production use is a separate effort requiring identity, edge, privacy, availability,
and security review. Completing this list alone is not a production-readiness guarantee.
