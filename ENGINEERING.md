# Engineering case studies

[Overview](README.md) · [Architecture](ARCHITECTURE.md) · [Evidence](PERFORMANCE.md) · [Reliability](RELIABILITY.md)

## 1. Reducing coordination overhead

### Engineering problem

An agentic request can spend substantial time outside model generation: selecting work, retrieving context, fitting prompts, waiting for tools, and checking results. Optimizing the model alone misses these costs.

### Why the problem is difficult

Latency accumulates across stages with different workloads. A faster route is useless if it drops a constraint, hides waiting work, or accepts an oversized context. Live desktop and inference activity also make simple before/after comparisons noisy.

### Constraints and tradeoffs

The system runs on a resource-constrained development workstation. Context is finite; current intent and execution requirements must survive compaction. Retaining more history improves inspectability but adds bookkeeping and context pressure.

### High-level design approach

Measure coordination subpaths independently. Reduce unnecessary historical-state inspection during idle recovery and keep context assembly bounded while preserving current requirements. Separate candidate recognition from execution authority so local handling can reduce unnecessary reasoning without granting new permissions. These are conceptual design decisions; selection logic and budget policies remain private.

### Verification methodology

[E2](PERFORMANCE.md#e2--idle-recovery-scan) uses matched ABBA ordering over actual coordinator/SQLite state. [E3](PERFORMANCE.md#e3--context-fitting) compares preserved baseline and modified prompt-fitting paths on retained offline workloads. Neither benchmark dispatches tools or invokes a model.

### Measured results

Recovery-scan medians fell from 57.53–60.39 ms to 1.09–1.23 ms in the final recorded ABBA benchmark. The discovery-heavy context changed from 0/20 accepted fits to 20/20; its median fitting time fell from 655.66 ms to 525.38 ms. These are repeated subpath measurements, not representative task completion results.

### Known limitations

Prompt replay repeats one constructed workload. Follow-up evidence reads and reasoning quality remain unmeasured. Background load prevents treating every timing difference as causal. No whole-system voice speedup is established, and arbitrarily large current requirements can still require an explicit budget refusal.

### Lessons learned

Measure the stage that changed. Distinguish computational speed, admission correctness, and end-to-end usefulness. Preserving failure evidence makes a narrow result more credible than a broad performance slogan.

## 2. Reliable workstation execution

### Engineering problem

A request can span a terminal session, project files, editor buffers, browser windows, and monitoring processes. The user expects coherent state across applications and later turns.

### Why the problem is difficult

Applications retain independent state and expose different control interfaces. An accepted command does not prove the intended resource exists. A restart can leave live processes behind; duplicate launches or cleanup against a stale identity can affect unrelated work.

### Constraints and tradeoffs

Preserve owned sessions and unsaved user work. Saved setup changes and immediate live changes have different meanings. Strict observation can produce conservative partial results when host interfaces cannot prove the requested state.

### High-level design approach

Keep versioned intended workstation state separate from live observations and historical memory. Coordinate dependent actions and inspect resource ownership before reuse or cleanup. Verify the effect at the relevant application boundary. Saving a definition records intent; reconciliation is responsible for observed setup state.

### Verification methodology

Tests inspect saved-definition persistence in a fresh process, competing updates, dependency validity, replay, and conflict handling. Recorded native probes use disposable application resources to check browser URLs, actual editor buffers, file-manager selection, and workstation capture/edit/read across coordinator reopen.

### Measured results

The selected records support qualitative scenario outcomes: saved changes persist across reopen; multiple requested editor buffers are observed; selected browser targets have matching observed URLs. They do not supply a representative multi-application task-success rate. The broad historical suite is reported separately as [E1](PERFORMANCE.md#e1--regression-suite).

### Known limitations

Browser URL evidence does not prove authenticated page usability. Arbitrary existing tabs, all workstation-edit paraphrases, and broad live speech with partial approvals remain incompletely validated. File-manager behavior can produce an additional view even when the requested selection is verified.

### Lessons learned

Make intended state, observed state, and evidence strength explicit. Idempotent behavior depends on trustworthy identity and current observations. Successful dispatch alone is too weak a completion criterion for a stateful workstation.

## 3. Persistent contextual memory

### Engineering problem

Useful continuity requires answering what happened in a project, resolving relevant references, and carrying verified outcomes into later tasks. A conversation archive alone cannot provide this reliably.

### Why the problem is difficult

Events can be stored long after they occurred. Similar names appear in different projects. Corrections can invalidate cached context. Semantic similarity finds related text but cannot establish identity, current truth, or causality.

### Constraints and tradeoffs

Scope and provenance must remain meaningful across retrieval paths. Bound context and storage work while retaining enough evidence to explain an answer. Prefer acknowledged ambiguity over a confident action against the wrong target. More indexing and correction handling impose ingestion and lifecycle costs.

### High-level design approach

Combine scoped semantic/lexical retrieval with structured historical evidence and current authorized observations. Distinguish event time from storage time. Preserve provenance, revision-aware freshness, and corrections. Use memory as evidence for reasoning; saved workflow authority and current execution authority remain in their own subsystems. The [memory diagram](ARCHITECTURE.md#memory-and-inference) shows this separation.

### Verification methodology

[E4](PERFORMANCE.md#e4--historical-retrieval) uses fixed project/time questions with separately specified expected source identities. Inspected tests also exercise local-calendar boundaries, ambiguity, project isolation, correction after restart, deletion, and context invalidation. Disposable Git/workflow integration tests connect observed outcomes to later evidence.

### Measured results

Exact source-set answers improved from 0/4 to 4/4 on the controlled ledger, with out-of-window sources reduced from five to zero. This is evidence of a specific temporal/source-selection correction, not general semantic or generated-answer accuracy.

### Known limitations

The ledger is small and constructed. Broad paraphrase resolution and generated-summary factuality remain unmeasured. Later lexical-latency acceptance missed its budget, so memory improvements do not imply a general performance win. Activity observations cannot establish unobserved work or human active time.

### Lessons learned

Context correctness is a lifecycle problem as much as a retrieval problem. Similarity, identity, chronology, and authority answer different questions. Small labelled fixtures are useful regressions when their population limits are explicit.

## 4. Local and remote inference

### Engineering problem

Reasoning, embeddings, and voice components compete for finite compute. Provider placement must balance responsiveness, capacity, privacy, and failure handling while retaining the same local execution boundaries.

### Why the problem is difficult

Changing inference location introduces network loss, partial streams, cancellation, backend availability, and outbound-context decisions. A transport retry can interact badly with already consumed output or subsequent tool effects. Multiple gateway clients do not prove the backend can sustain parallel inference.

### Constraints and tradeoffs

Local execution limits data movement but competes with desktop and voice workloads. Remote placement offers capacity at the cost of transport and deployment complexity. Streaming improves responsiveness while complicating cleanup and completion detection.

### High-level design approach

Use a provider abstraction for local and optional remote inference workloads. Keep model serving separate from workstation execution. Bound requests and manage connection lifetimes; require explicit configuration for outbound context. Backend serving infrastructure owns GPU scheduling. The inference gateway is an integration boundary, not a new general-purpose agent authority.

### Verification methodology

Inspected provider/gateway tests use simulated backends to exercise invalid responses, authentication/resource boundaries, streaming, disconnect cleanup, and failures. Selected local native scenarios use the installed model. Current remote operation is not certified by the comprehensive audit.

### Measured results

No remote throughput, sustained multi-client capacity, or cross-machine latency figure is promoted. The verified contribution is implemented separation of inference and execution responsibilities with bounded transport tests. Context-fitting measurements in E3 concern local assembly, not inference tokens per second.

### Known limitations

Current remote cancellation under saturation, long-run connection recovery, GPU contention, and out-of-memory behavior remain open validation areas. Optional shared admission support does not establish a verified distributed deployment. Provider compatibility is narrower than arbitrary tool-enabled streaming.

### Lessons learned

An inference abstraction must preserve failure semantics as well as response shape. Evaluate provider transport, model serving, and workstation effects separately before claiming distributed system performance.

## 5. Securing tool-enabled execution

### Engineering problem

Natural-language interpretation must not turn model confidence, retrieved instructions, or remembered context into unrestricted workstation authority.

### Why the problem is difficult

The target can change between interpretation and execution. Tasks can be interrupted while effects remain in flight. Repeated delivery can produce duplicate effects, and fluent model output can overstate what actually happened.

### Constraints and tradeoffs

Maintain user approval boundaries across worker processes, saved state, and recovery. Preserve enough durable evidence to explain incomplete work while keeping operational material private. Conservative validation can require clarification or report a partial result even when the model expects success.

### High-level design approach

Separate recognition, planning, authorization, effects, and verification. Validate against current state, retain durable outcomes, and use ownership-aware recovery. Keep approval state distinct from successful execution. Describe completion according to observed evidence rather than generated assertions. The [recovery diagram](RELIABILITY.md#failure-and-recovery-flow) illustrates these responsibilities without exposing enforcement rules.

### Verification methodology

Inspected tests cover controller execution, approval waiting, identity/scope mismatches, replay and stale work, conflict coordination, cancellation, and process restart. Selected recovery tests perform real disposable file operations and verify later state. Native probes complement unit tests where application observations matter.

### Measured results

These controls have bounded regression evidence within the recorded suite and focused integration records. No universal exploit-prevention percentage, recovery success rate, formal proof, or independent penetration-test result is claimed.

### Known limitations

External applications do not participate in a global transaction. Some interrupted effects are uncertain and cannot be blindly retried. Voice recognition does not establish privileged authority. New integrations and untested combinations can introduce failure modes outside existing coverage.

### Lessons learned

Authority is a current systems property; interpretation is a fallible estimate of intent. Durable outcomes and honest partial completion are practical foundations for recovery, but they do not create a universal exactly-once guarantee.
