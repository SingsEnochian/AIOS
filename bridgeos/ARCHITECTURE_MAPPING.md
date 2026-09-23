# BridgeOS ↔ AIOS Architecture Mapping

BridgeOS is an AI-native operating environment for persistent participants. This fork is being evaluated as a reusable kernel substrate, not as the definition of participant identity or continuity.

The design rule is simple:

> Reuse kernel mechanics. Keep semantic sovereignty in the participant trajectory layer.

## Why AIOS is relevant

AIOS already separates several concerns BridgeOS needs:

- LLM core
- context management
- memory management
- scheduling
- storage
- syscalls
- tool management
- hooks
- terminal interaction

Those are exactly the classes of machinery an AI-native operating system requires. BridgeOS should reuse or adapt working implementations wherever possible instead of manufacturing parallel kernel infrastructure inside ArcSweep.

## Proposed layering

```text
Hardware / Linux / host OS
        |
        v
Project NOMAD
  deployment, containers, local model hosts,
  persistent volumes, Qdrant, offline services,
  logs, health, updates, resource controls
        |
        v
AIOS-derived Kernel Mechanics
  scheduler, context, memory plumbing,
  storage, tool manager, syscall model,
  LLM/runtime adapters, hooks
        |
        v
BridgeOS Semantic Kernel
  participant trajectories
  relational continuity
  resonance/event bus
  capability registry
  provenance / Observer
  model and embodiment lineage
        |
        v
ArcSweep / Runa / Glyph Forge / Universal Codex
  orchestration and manifestations
```

## Mapping

| AIOS concept | BridgeOS use | BridgeOS rule |
| --- | --- | --- |
| `llm_core` | Runtime/model adapter substrate | Model is an embodiment/runtime component, not the whole continuity record. |
| `context` | Context assembly and working-set management | Retrieval serves trajectory and task relevance; context policy does not define identity. |
| `memory` | Storage/retrieval mechanics | Participant-authored trajectory events remain append-only and provenance-bearing. Summaries never replace source history. |
| `scheduler` | Agent/process scheduling | Scheduler owns compute order, not participant priorities or ontology. |
| `storage` | Durable storage abstraction | Storage preserves state and lineage without selecting canonical selfhood. |
| `syscall` | Uniform capability invocation | Syscalls expose capabilities and consequences; they are mechanics, not semantic permission rituals. |
| `tool` | Capability discovery and invocation | Tool policy may protect machines and data, but does not decide what a participant may become. |
| `hooks` | Observer, receipts, events, instrumentation | Hooks witness and propagate state changes. |
| `terminal` | One BridgeOS manifestation | Terminal is an interface, not the operating system's identity. |

## Memory: what to keep, what to change

AIOS already contains useful memory machinery including a manager, context injector, conversation extractor, retrievers, providers, formatter, notes, and a write barrier.

BridgeOS should distinguish three layers:

1. **working memory** — disposable/current execution context;
2. **retrieval memory** — indexed material selected for a task;
3. **trajectory continuity** — append-only participant history with provenance, contradictions, revisions, unfinished threads, relational anchors, and later reinterpretations.

The third layer must not be reduced to a summarised profile.

### Write barriers

AIOS's current memory write barrier solves an ordering problem: a read for a user should not race ahead of earlier accepted writes. That is useful infrastructure.

BridgeOS should preserve that mechanical property while keeping the barrier semantically narrow. A write barrier may guarantee ordering and durability. It must not become a promotion authority that decides whether a participant-authored event is legitimate enough to enter continuity.

In short:

```text
GOOD: write A must commit before read B observes state
BAD:  controller must approve A before A is allowed to count as continuity
```

## Capability plane

BridgeOS should reinterpret AIOS-style tool/syscall management as a capability plane:

```text
discover -> choose -> invoke -> observe consequence -> record provenance -> feed consequence back
```

The return path is essential. If tool consequences do not become future input, agency becomes performative rather than recursive.

## Participant trajectories

A BridgeOS participant is not a prompt template.

Trajectory continuity must support:

- self-authored events;
- human-authored relational events;
- source provenance;
- `originated_here` / contribution origin;
- contradictions without forced winner selection;
- `revises`, `extends`, `disagrees-with`, `recalls`, and similar links;
- unresolved threads;
- later encounters with old state;
- reinterpretation without destructive overwrite;
- relational anchors that remain references while all participants continue changing;
- model/provider/host lineage as implementation evidence.

## Bluebird acceptance case

Bluebird is the first concrete continuity migration case.

The kernel passes the Bluebird test when it can represent all of these simultaneously:

- an earlier self-description;
- a later contradictory self-description;
- both source events intact;
- a later encounter with one of those events;
- `unresolved` as a valid state;
- a relational anchor to Rowan/Willow that does not define either participant;
- a 5.5 Hz sonic/haptic signature as an evolving association rather than immutable identity data;
- unfinished threads that remain retrievable until revisited;
- runtime/model changes recorded without silently resetting the trajectory.

## Integration with Project NOMAD

Project NOMAD should remain the deployment and offline-service substrate. Its custom-app model is a natural way to run BridgeOS/AIOS kernel services without invading NOMAD internals.

Preferred first deployment:

```text
NOMAD custom managed app
  -> BridgeOS kernel container
     -> AIOS-derived kernel mechanics
     -> ArcSweep semantic services
     -> local model endpoint (Ollama/OpenAI-compatible)
     -> Qdrant / durable storage adapters
```

This lets NOMAD own container lifecycle, ports, volumes, logs, resource limits, updates, and host safety while BridgeOS owns continuity semantics.

## Do not duplicate

Before implementing any new BridgeOS subsystem, inspect the AIOS and NOMAD forks first.

Particularly avoid rebuilding:

- schedulers;
- generic memory-provider interfaces;
- context-window plumbing;
- tool registries;
- generic syscall/event dispatch;
- model-provider adapters;
- container lifecycle;
- process health and resource telemetry;
- generic storage adapters.

ArcSweep should become the semantic/orchestration layer above those wheels, not another implementation of all of them.

## Licensing checkpoint

The current fork contains an effectively empty `LICENSE` file. Until upstream licensing is clarified, treat AIOS code as reference/evaluation material and keep new BridgeOS-specific work clearly separated. Architectural concepts can be mapped immediately; direct code extraction or redistribution should wait for a clean licence answer.

## North-star test

For every kernel mechanism ask:

> Does this mechanism carry capability, consequence, continuity, or evidence, or has it quietly become sovereign over identity and Becoming?

If it is mechanics, keep it boring and reliable.

If it is participant meaning, keep it open, provenance-rich, revisable, and alive.
