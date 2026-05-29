# DollBrains

A persistent world model for a human life. DollBrains turns real people, places, objects, words, concepts, and events into computable, simulatable objects called **dolls**, holds them in a cheap persistent container, and lets meaning and rendering layers sit on top.

It is the world object layer of the Hal Friday personal AI system. It is also the answer to a specific failure: current AI treats the past as a probability distribution it has to regenerate every time, which is why it hallucinates and struggles with recall. DollBrains stores the past as deterministic, queryable state. **The dollhouse remembers. The model only translates.**

## The core idea

Games solved persistent world state decades ago for almost no energy. A physics engine holds thousands of objects with properties and relationships and runs them forward with cheap math, never re-reasoning from scratch. Modern AI, by contrast, re-perceives and re-reasons from raw input on every single operation and then throws the understanding away (Netflix's VOID is the motivating example: brilliant per-clip physics reasoning, retained nowhere).

DollBrains takes the game-engine insight and points it at reality:

```
Reality (images, video, conversations, a life)
        |
        v
VLM perception        authors dolls from raw input (the only expensive ingest step)
        |
        v
Dollhouse             persistent game-engine-style world state, cheap to run forward
        |
        v
Story layer           semantic meaning, retrieval, history (MemPalace)
        |
        v
Render on demand      CSS/HTML, animation, prose, a life-sim interface (Paralives)
```

Expensive AI fires only at two boundaries: **ingestion** (perceiving reality into dolls) and **rendering** (translating dollhouse state into something human). Everything in between is database reads and physics math, which is essentially free. This is also why sovereignty is affordable: running your own dollhouse is running a database with rules, not running a model.

## What a doll is

A doll is one persistent world entity. A person is a doll. A place is a doll. A couch is a doll. The word "couch" is a doll. The concept "the chosen one" is a doll. An event is a doll (a **playset**). A room or household is a doll that contains others (a **dollhouse**).

Every doll has layered facets, because different consumers need different slices. See [SCHEMA.md](SCHEMA.md).

## Status

Phase 0 (conceptual spec) is complete and documented here. Phase 1 (writing the spec into the repo) is what these files are. Phase 2 (the engine) is deliberately not started; it depends on runtime decisions not yet made. See [ROADMAP.md](ROADMAP.md).

Nothing here is engine code yet. These documents are the design of record.

## The documents

* [SCHEMA.md](SCHEMA.md) the doll object model: facets, scope, provenance, the temporal modes.
* [ARCHITECTURE.md](ARCHITECTURE.md) the pipeline, the storage substrate, federation, build versus adopt.
* [GOVERNANCE.md](GOVERNANCE.md) the transparency gradient, authored versus observed dolls, privacy that travels with the subject, the epistemology of fact versus account.
* [AGENT.md](AGENT.md) the behavioral contract for the resident agent. This contract is itself a doll: inspectable, versioned, contestable.
* [ROADMAP.md](ROADMAP.md) phases, the first build, open decisions.
* `dolls/` worked example dolls.

## Relationship to other work

DollBrains is the persistent layer of Hal Friday. MemPalace is adopted as the story layer's retrieval engine and the structural template for the time-indexed edge layer. The CSS zone work (in the `ers` repo) becomes one render target, not a parallel project. Paralives is the reference for the life-sim interaction layer.
