# Authorship, dependencies, and scope

[Overview](README.md)

IntentOS combines project-specific coordination, integration, persistence, and verification work with external frameworks, model runtimes, and pretrained models. This showcase describes engineering contributions at the system level. It does not claim authorship of external software, model architectures, pretrained weights, or research.

Repository metadata reviewed privately is insufficient to establish sole authorship or a complete contributor roster. Personal names, private account identifiers, and private commit history are intentionally absent. A named maintainer/byline can be added after the owner confirms the intended public attribution.

| Technology or ecosystem | Role observed in the private implementation |
| --- | --- |
| Python and Pydantic | Runtime implementation and structured validation |
| LangChain and LangGraph | Model integration, agent state and checkpointing |
| Ollama | Local generative model serving |
| ONNX Runtime and tokenizers | CPU semantic inference |
| FastAPI, HTTPX and optional Redis | Optional inference gateway and transport/admission infrastructure |
| SQLite and PostgreSQL integrations | Local persistence and optional orchestration/checkpoint backends |
| Sentence Transformers, Transformers, PyTorch and Nomic models | Embeddings, optional reranking, and model experimentation |
| Hindsight and LangMem integrations | Optional memory-related services and processing |
| openWakeWord, faster-whisper, SpeechBrain, Silero VAD, Piper and Kokoro | Voice processing integrations and optional speech components |
| Qt/PySide, i3, tmux, Kitty, Neovim and Linux accessibility interfaces | Voice feedback and native workstation integration |
| Qwen model family | Local reasoning in the reviewed runtime evidence |

The table indicates inspected integration or declared dependency, not universal activation or measured quality. Optional dependencies may be absent in a given installation. All names remain the property of their respective owners. No affiliation or endorsement is implied, and no third-party implementation or model artifact is redistributed here.

The [rights notice](LICENSE) applies only to this documentation collection. It does not license IntentOS software or grant rights in third-party materials. No Creative Commons or open-source software license has been adopted without owner confirmation.
