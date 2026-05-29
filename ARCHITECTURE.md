# Architecture

## The pipeline

```
Reality (images, video, conversations, Day One, Way Back Machine, a physical book)
        |
        v
[ INGEST ]  VLM perception authors dolls and edges from raw input.
            The only expensive ingest step. Inspired by Netflix VOID's
            VLM-mask-reasoner, except the reasoning is kept, not discarded.
        |
        v
[ DOLLHOUSE ]  Persistent game-engine-style world state. Objects with
               properties, constraints, typed edges. Runs forward with
               cheap math. Each doll is its own save state.
        |
        v
[ STORY ]  Semantic meaning, retrieval, history. MemPalace.
        |
        v
[ RENDER ]  On demand only. CSS/HTML, animation, prose, a life-sim
            interface. The model translates dollhouse state here; it
            never invents state.
```

Expensive AI fires only at ingest and render. Everything between is database reads and physics math.

## Why the game-engine model is the cheap persistence layer

Game physics engines (Havok, PhysX, Bullet) hold each object as a small record (mass, collision mesh, friction, velocity, a few floats) and run constraint solving forward each frame as matrix math. Thousands of interactions at 60fps for fractions of a watt. The expensive part of a game is authoring the world; the simulation is nearly free.

VOID demonstrated that a VLM can automate the authoring step (look at pixels, produce relational and causal reasoning). DollBrains keeps that authored understanding as persistent dolls instead of throwing it away. The synthesis: VLM-automated perception, persistent game-engine world state, cheap forward simulation, and the expensive generative model invoked only to render.

### How games avoid re-evaluating everything (the answer to "won't this re-reason constantly?")

Games do re-evaluate per frame, but never everything. Five tricks, all of which map to DollBrains:

1. **Sleeping bodies.** An object at rest is skipped until perturbed. A doll untouched for months sits as a compressed reference and is not reasoned about until something touches it.
2. **Spatial partitioning.** The engine only checks plausibly-near objects. A query about the kitchen does not load the yearbook. Scope by dollhouse, by playset, by a few hops of `company` edges.
3. **Dirty flags.** Derived values are cached and only recompute when their source changes. The "current" value of a facet is cached; it rederives only when an upstream doll changes and propagates invalidation. Most queries read cache.
4. **Event propagation, not polling.** A changed doll publishes to its dependents along edges; nothing else recomputes.
5. **Level of detail.** Out-of-scope dolls return summaries, not full facet trees. Fidelity scales with attention.

So contextual objects are not reassessed every time anything is looked at. Reads hit cache. Writes ripple only to actual dependents. Expensive reasoning fires only for genuinely new or genuinely perturbed dolls. What VOID lacks is precisely the cache and the edges; those are DollBrains' missing middle.

## Storage substrate (not blockchain)

The requirement is persistent, continuously updatable, no central data center, no water-and-power waste, shareable, verifiable. Blockchain fails most of these: it is bad at storing data (everything replicated forever), it is immutable where we need updatable, and proof-of-work is the energy disaster we are escaping. The blockchain reach was a reach for "shared verifiable state," and the real tools for that are three separate things:

1. **Content addressing** a doll id is a content hash (like git). Makes canonical truth verifiable and shareable: two people referencing the same book doll provably reference the same bytes. Designing doll ids as content hashes from day one keeps everything below from being locked out.
2. **Local first plus CRDTs** each dollbrain lives on hardware the owner controls, works fully offline, and syncs peer to peer, opt in. CRDTs (conflict-free replicated data types) let two copies merge without a central authority. This is the real answer to "continuously updatable, no data center."
3. **Tamper-evident audit logs** append-only, hash-linked logs bolted onto institution dolls only, so an institution cannot rewrite its own public history. Not a global chain. A per-institution log anyone can verify.

The storage model is **personal server plus selective sync.** Only `published` facets ever leave the machine. Sync needs only dumb, untrusted transport (the relay cannot tamper because data is content addressed, and never sees private data). It does not need shared compute. The privacy gradient and the storage architecture are the same shape: humans get local-first private storage that syncs when they choose; institutions get the unforgeable public ledger they cannot escape.

## Federation and nested public dolls

A place doll is also a dollhouse, and they nest all the way up. Scope flips at the front door: private and authored below, public and observed above. The commons is the shared top of the tree (Earth, a nation, a state) made of observed dolls nobody owns, synced peer to peer. You plug your private stack into the bottom of the public one. That is federation. See [GOVERNANCE.md](GOVERNANCE.md) for authored versus observed dolls.

## Build versus adopt (MemPalace)

MemPalace (`../mempalace`) was inspected. Findings:

**Why its recall is high (four portable choices):**

1. It never lossy-compresses content. Verbatim storage is the foundational promise; the AAAK compression is only the index layer, never the content, so compression cannot cost recall.
2. **The index is a signal, never a gate.** In its `searcher.py`, direct drawer search is the floor and always runs; the compressed index can only boost ranking, never hide a hit. This is exactly the principle DollBrains' divergence-preservation law requires: a ranking heuristic must never hide a divergent account.
3. Hybrid retrieval (BM25 lexical plus vector semantic), over-fetch then re-rank, with a union mode that catches lexically strong but semantically distant documents.
4. Recall survives infrastructure failure via a SQLite FTS5 BM25-only fallback. Its dense fixed-bug history is hard-won value.

**The piece closest to DollBrains:** `knowledge_graph.py` is a SQLite triple store, `(subject, predicate, object)` with `valid_from`/`valid_to`, `as_of` time-filtered queries, and fact invalidation. A typed predicate is a `company` edge. valid_from/valid_to is `playthrough` time-indexing. An `as_of` query is "query the past as deterministic state at timestamp T", the core thesis, already working in code.

**The calls:**

* **Adopt the retrieval engine wholesale.** Rebuilding would reintroduce fixed recall bugs. The boost-not-gate principle is required, not just convenient.
* **Adapt the temporal KG with one surgical change.** MemPalace assumes one truth per fact (a new value closes the old via `valid_to`). DollBrains must hold contradiction (the witness layer). Stop auto-closing `valid_to` on conflict, allow multiple live object-values per subject-predicate, add the `scope` and corroboration fields, and make the old-to-new relationship an explicit typed edge (the four temporal modes). This extends MemPalace's verbatim and never-delete values; it does not violate them.
* **Build new:** the doll object schema, the forward-simulation game-engine layer, the scope and transparency machinery, content-addressed doll ids and local-first sync, corroboration plus independence weighting, the agent behavioral layer.
* **Free win:** MemPalace has a pluggable backend interface (`backends/base.py`), so DollBrains storage can attach without forking the core, and it already exposes everything over MCP.

One line: MemPalace is the story layer's retrieval engine and the structural template for the playthrough and edge layers. The hard, bug-riddled part (high-recall retrieval, time-filtered triples) is done. The new part is the world model on top.
