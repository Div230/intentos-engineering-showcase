# Capabilities and validation boundaries

[Overview](README.md) · [Architecture](ARCHITECTURE.md) · [Limitations](LIMITATIONS.md)

Status refers to the inspected implementation and existing evidence, not a production deployment certificate. Historical suite results are identified in [performance evidence](PERFORMANCE.md).

| Area | Status | Evidence and boundary |
| --- | --- | --- |
| Natural-language interpretation | Implemented; selected paths verified | Deterministic resolution, CPU semantic matching, model-assisted interpretation and clarification; broad language accuracy unmeasured |
| Structured multi-action planning | Implemented; bounded tests verified | Dependency and conflict tests, scope checks and durable results; compound live voice requests with partial approvals lack broad acceptance evidence |
| Local inference | Implemented; selected live paths verified | Ollama-backed reasoning and CPU inference observed; no sustained saturation or whole-system latency claim |
| Remote/hybrid inference | Implemented; current integration validation incomplete | Provider/gateway tests exercise transport and policy using simulated backends; current cross-machine load, recovery and cancellation unverified |
| Streaming and cancellation | Implemented; bounded tests | Streaming cleanup and task interruption paths exist; distributed cancellation under contention remains open |
| Persistent scoped memory | Implemented; bounded tests verified | Scope, correction, deletion, freshness and restart tests; unrestricted semantic QA unmeasured |
| Temporal/activity memory | Implemented; controlled fixtures verified | E4 checks project/time source selection; installed-service backfill and large-scale history acceptance not established |
| Workspace retrieval | Implemented; tests inspected | Catalogue, lexical/vector retrieval, optional reranking and bounded context; no public representative quality benchmark |
| Saved workstations | Implemented; selected native scenarios verified | Versioned definitions, restart persistence, dependency validation and reconciliation; exhaustive CRUD language coverage incomplete |
| Terminal/window/browser coordination | Implemented; selected native scenarios verified | Owned-resource tests and actual browser URL/window evidence; authenticated page usability and arbitrary tabs remain limited |
| Files and Git-aware work | Implemented; bounded integration verified | Project discovery, editor/file-manager probes, disposable local Git and memory-relay tests; authenticated remote history ingestion incomplete |
| Approval and execution verification | Implemented; bounded tests verified | Controller, receipt identity, replay, ownership and result verification tests; no universal safety or formal proof claim |
| Recovery | Implemented; bounded tests verified | Restart/cancellation and selected file-recovery tests; arbitrary external effects are not transactionally reversible |
| PostgreSQL persistence | Implemented in parts; current server validation incomplete | Orchestration/checkpoint support exists; activity-memory queries are SQLite-specific |
| Compact trained semantic parser | Experimental | Validation generalization is insufficient; distinct from runtime CPU capability matching |
| Sustained-load and representative speech evaluation | Planned validation work | No completed broad acceptance result found in the reviewed evidence |

## Vexon and IntentOS

Vexon supplies audio capture, wake processing, speaker verification, transcription, conversation state, approval interaction, visual feedback, speech output, and interruption handling. Transcribed requests enter IntentOS coordination; the workstation action controller remains responsible for execution authority.

The inspected stack includes openWakeWord-based processing, faster-whisper transcription, SpeechBrain speaker verification, Piper/Kokoro speech options, and Qt-based feedback. Presence of an integration does not establish robust wake recognition, speaker isolation, or intelligible audio under real environmental noise. Voice recognition is not presented here as sufficient authorization for privileged execution.

Conversation continuity and interactive approvals have regression coverage. Physical microphone-to-effects acceptance, measured false activation/rejection rates, real interruption quality under inference contention, and human evaluation of spoken responses remain incomplete.
