# Measured engineering evidence

[Overview](README.md) · [Case studies](ENGINEERING.md) · [Limitations](LIMITATIONS.md)

All results below come from existing private engineering records inspected on 2026-10-23. Application tests and benchmarks were not rerun for this documentation release. The original system was kept unchanged. Public summaries omit raw logs, requests, host identity, and operational details.

Evidence labels identify specific records, not independent third-party certifications. Exact artifact locations, hashes, measurement checks, and source mappings are retained in a separate private review packet. Public readers can assess the methods and scope here; they cannot independently rerun private implementation benchmarks from this repository.

## E1 — Regression suite

**Source:** comprehensive runtime audit, acceptance full-suite record and associated source manifest, 2026-10-22. **Snapshot:** historical working tree, with uncommitted development changes; a private commit alone does not identify it.

| Outcome | Count |
| --- | ---: |
| Passed | 2,232 |
| Skipped | 55 |
| Failed in this run | 0 |
| Warnings | 12 |
| Wall time | 225.66 s |

**Method/environment:** repository pytest suite in the development Python/Linux environment with installed optional dependencies. Tests mix isolated fixtures, mocked services, and integration paths; live and optional prerequisites can cause skips. Full per-test machine/dependency capture is not established by the console summary.

**Limits:** earlier runs recorded failures; a later acceptance run does not erase them. The inspected source differs in 18 of 382 entries from the acceptance manifest, with no missing entries. This result is therefore historical, not a pass claim for the current tree. Counts from overlapping suites are not added. Passing tests do not measure production reliability, task success probability, or coverage percentage.

## E2 — Idle recovery scan

**Source:** comprehensive runtime audit, final recovery benchmark, 2026-10-22. **Snapshot:** historical final benchmark variant compared with a restored prior candidate-selection path.

| ABBA arm | Warm samples | p50 | p95 | Process CPU time for sampled loop |
| --- | ---: | ---: | ---: | ---: |
| Baseline A | 30 | 60.39 ms | 66.72 ms | 1.8034 s |
| Modified B | 30 | 1.23 ms | 1.29 ms | 0.0360 s |
| Modified B | 30 | 1.09 ms | 1.17 ms | 0.0334 s |
| Baseline A | 30 | 57.53 ms | 64.76 ms | 1.7523 s |

**Method:** actual coordinator with disposable SQLite state containing 150 blocked records; one warm-up per arm, then elapsed-time samples. Baseline/modified/modified/baseline ordering reduces simple ordering bias. No actions, retrieval calls, or model calls execute. The baseline restores the prior selection behavior inside the same harness; it is not a separate historical deployment.

**Environment:** same local Linux development workstation; accompanying audit reports an Intel i7-13650HX, about 16 GiB host RAM, and RTX 4050 Laptop GPU. This workload does not use GPU inference. Background desktop/test activity was present; complete isolation was not established.

**Limits:** roughly 98% lower median scan time describes this bookkeeping workload only. The final raw artifact supersedes different numbers in the earlier prose performance table. Small-sample p95 uses the recorded sorted-sample index convention; no p99 or long-run idle CPU claim is made.

## E3 — Context fitting

**Source:** comprehensive runtime audit, prompt-budget benchmark, 2026-10-22. **Snapshot:** historical preserved baseline and modified context-fitting implementations.

| Offline workload | Warm trials per variant | Successful fits before → after | p50 before → after | p95 before → after |
| --- | ---: | ---: | ---: | ---: |
| Initial assembled context | 20 | 20/20 → 20/20 | 113.39 → 52.22 ms | 149.00 → 58.47 ms |
| Discovery-heavy context | 20 | 0/20 → 20/20 | 655.66 → 525.38 ms | 742.53 → 665.18 ms |

**Method/environment:** offline assembly from a retained task and recorded discovery groups on the same development workstation described in E2; first iteration excluded from warm statistics. No model generation, retrieval, or effects. The discovery workload is a constructed replay of retained evidence, not a byte-identical capture of an entire failed inference request.

**Limits:** twenty trials repeat each fixed workload; they are not twenty different tasks. Fit success means the budget accepts the context, not that the model completes the task. Later evidence reads, reasoning latency, physical voice latency, arbitrary oversized requests, and output quality were not measured. Sequential before/after execution with background load does not establish controlled causal speedup.

## E4 — Historical retrieval

**Source:** relational/temporal memory audit, comparison and fixed activity-ledger benchmark records, 2026-10-21. **Snapshot:** saved baseline modules versus the audit's modified implementation.

| Fixed ledger outcome | Before | After |
| --- | ---: | ---: |
| Queries returning the exact expected source set | 0/4 | 4/4 |
| Returned sources outside the requested time window | 5 | 0 |

**Method:** four project/time queries over eight controlled events across two projects, with 1,000 irrelevant lexical memories. Event occurrence and storage times intentionally differ. Expected source identities are specified separately from retrieval logic. This evaluates source selection, not generated summaries.

**Environment:** disposable local SQLite fixtures in the development Python environment. A complete isolated hardware inventory is not encoded in the selected comparison artifact; do not compare its timings against other workloads.

**Limits:** the corpus is too small and constructed to establish real-world retrieval precision or natural-language QA accuracy. The same audit's later separate-process lexical-latency comparison missed its acceptance budget. Stable-host latency sign-off remains open; memory quality results do not imply a performance win.

## Evidence that remains qualitative

Recorded native probes exercise browser URL observation, multiple editor buffers, file selection, workstation saved-state changes across coordinator reopen, and a persisted visual reminder. These are scenario checks rather than a representative task-success dataset. Remote inference latency, sustained throughput, cold/warm model generation, physical voice performance, long-run memory growth, and recovery rates under production load lack adequate current evidence for a public numerical claim.
