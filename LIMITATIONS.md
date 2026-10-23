# Limitations and open engineering work

[Overview](README.md) · [Measurements](PERFORMANCE.md) · [Capabilities](CAPABILITIES.md)

## Evidence limits

The acceptance suite is historical: the current inspected tree has diverged from its recorded manifest. Skips include environment-dependent paths. Existing unit and integration tests do not establish deployment reliability, representative task accuracy, formal security, or usability.

The performance evidence measures particular subpaths. It does not establish a target reduction in whole-system voice latency, a broad multi-action completion rate, distributed throughput, or memory usage under sustained load. The memory audit's later lexical-latency acceptance budget was missed; stable-host sign-off remains unresolved.

## Functional boundaries

- **Context and language:** ambiguous references can require clarification. Cross-turn historical navigation, arbitrary multi-action phrasing, and partial approval in the full live voice stack lack broad acceptance coverage.
- **Desktop state:** correctness depends on host interfaces and observable application state. A visible browser window or matching URL does not establish usable authenticated page content. Exact ownership of arbitrary existing tabs is incompletely supported.
- **Voice:** wake and speaker components exist, but robustness against real noise, overlapping speakers, spoofing, and sustained resource contention is not established by the reviewed evidence.
- **Recovery:** selected owned resources and file operations have recovery tests. There is no global rollback across external applications, no universal exactly-once side-effect guarantee, and no assurance that an interrupted effect did not occur.
- **History:** recorded activity is partial evidence. Execution duration is not human active-work duration; temporal proximity is not causal proof. Missing observations cannot be reconstructed as facts.
- **Memory lifecycle:** application-level erasure does not prove removal from independent backups, storage remnants, or other services. Large-scale growth, retention across all stores, and installed-service backfill need separate evaluation.
- **Inference and persistence:** current remote saturation/cancellation and PostgreSQL server execution remain incompletely verified. Local SQLite activity queries are not a portable database implementation.
- **Observability:** a recorded desktop accessibility warning stream remains unresolved. Its performance impact was not isolated.
- **Learned semantics:** the compact parser remains research work. Per-field training metrics are insufficient evidence of complete request interpretation correctness.

## Next validation work

Use representative, labelled task and audio workloads; measure end-to-end outcomes separately from routing and context assembly. Repeat memory latency measurements on a stable host, exercise service restarts and remote cancellation under load, and evaluate ambiguity and retrieval quality across realistic projects. These are proposed validation priorities, not completed achievements or promised product features.
