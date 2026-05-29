# Roadmap

## Phases

* **Phase 0, conceptual spec.** Complete. Captured in [SCHEMA.md](SCHEMA.md), [ARCHITECTURE.md](ARCHITECTURE.md), [GOVERNANCE.md](GOVERNANCE.md), [AGENT.md](AGENT.md).
* **Phase 1, write the spec into the repo.** In progress. These documents are it. No engine code, no tech-stack lock-in. Pure transcription of settled decisions.
* **Phase 2, the engine.** Not started, deliberately. It depends on runtime decisions not yet made (below).

## The first build (when Phase 2 opens)

The solar system is the smallest complete and fully verifiable instance of the entire architecture, so the first worked doll is a **planet doll backed by a real ephemeris**.

Why it is the right first build:

* A closed set of persistent objects (the bodies). Object permanence for free.
* Cheap forward simulation that never re-reasons: orbital mechanics is the game engine. Positions are computed, not generated, and identical every time. This is the past as deterministic state, the core thesis, in a domain where every answer is checkable against the sky.
* Mathematical relation held continuously: aspects are angular `company` edges, pure functions of position.
* Time contextuality that never collapses: an ephemeris is a `playthrough`. A natal chart is the playset frozen at a birth timestamp; transits are the same data at two timestamps.

It also splits perfectly along the schema seam:

* **Positions are `canonical`, `established`.** Pure astronomy, verifiable, identical for everyone. No belief required.
* **Meaning is `private`, in `story`.** What a placement means or feels like, linked to MemPalace. Sovereign.

You never have to make anyone believe astrology. The positional engine is just astronomy and it is true; the interpretive layer is yours.

Shape of the first build:

* a planet doll backed by an existing ephemeris library (no VLM needed; the sky is already digitized)
* positions as canonical timed facets
* aspects as computed `company` edges between planet dolls
* a natal dollhouse: the chart frozen at a birth timestamp
* an empty `story` layer ready for personal meaning

The natal chart doubles as an **onboarding interface** for other people building their own dollbrains: "what is your sun sign" is a tractable first question whose answer becomes a real doll. Houses are dollhouses scoped to life domains; planets are standing self-facet dolls; aspects are a prebuilt edge vocabulary; transits and progressions are longitudinal by construction.

## Open runtime decisions (block Phase 2)

These are deliberately unmade. They need real consideration, not a default.

1. **Embedding model and local model choice.** Which runtime holds the context and honors the agent contract. Likely a fine-tuned personal model rather than a cloud model; this is the central hard problem.
2. **Ephemeris library.** Which astronomy library computes positions for the first build.
3. **Content-addressing scheme.** How doll ids are hashed, so federation is not locked out later.
4. **The MemPalace KG fork.** Exactly how the temporal graph is adapted to hold contradiction (stop auto-closing `valid_to`, multiple live values per subject-predicate, the typed temporal-mode edge, scope and corroboration fields). Whether to attach via its pluggable backend interface or fork.
5. **Storage format for v0 dolls.** JSON files per doll (git-friendly, content-addressable) versus SQLite (relational joins cheap) versus both (JSON source of truth, SQLite derived index).

## Sequencing principle

Build the prototype as Magpie's own dollbrain first. Then generalize into an onboarding flow for others (file ingestion, guided questions, web-history pull, real-world sight for physical objects and books). Federation (publish and follow) is v2. The tamper-evident institutional audit logs and the cause-gate governance are v3. Each person owns their dollhouse; running it is running a database with rules, not running a model.

## Naming decisions (called, reversible)

* The history facet is named `playthrough`.
* The first worked doll is the Sun / planet doll backed by an ephemeris.
