# The Doll Schema

A doll is one persistent world entity. Everything is a doll: people, places, objects, words, concepts, tropes, events. The schema is the same for all of them; facets that do not apply are simply empty (a concept doll has no `body`).

## Facets

A doll has nine facets. Facets are not flat values. Where a facet refers to another thing, it holds a **pointer to another doll**, not an inline value, so the structure bottoms out only at atom dolls (literals: a timestamp, a phoneme, a color hex, a coordinate).

| Facet | What it holds |
|-------|---------------|
| `self` | Identity, editable. Names, aliases, `kind`. `kind` is an open string, not an enum, because a couch, a person, a word, and a concept are all dolls. Gender lives here, as a pointer to the gender concept doll, so it can be editable and longitudinal. |
| `body` | Physical form: bounds (bbox or mesh ref), position, scale, mass, material, anchored-to. Empty for concept and word dolls. |
| `wardrobe` | Swappable appearance and state: pose, mood, condition, on/off, currently-held-by, clothes, skin. Cheap to change. |
| `play` | Affordances and interaction rules: what the doll does, what can be done to it, physics roles (rigid, soft, fluid, light source, container). This is where game-AI rules live. |
| `catalog` | The possibility space: what the doll could do, could become, could contain that has not been realized. A TV's channel guide. A word's possible meanings. A person's potential selves. Distinct from `play` (realized rules) because catalog is unrealized options. |
| `company` | Typed edges to other dolls: `contains`, `supports`, `owns`, `knows`, `lives_in`, `parent_of`, `made_from`, `paired_with`, `references`, and so on. The graph. |
| `story` | Links into the meaning layer (MemPalace): closet ids, drawer ids, diary entries where this doll appears. The meaning of the doll. |
| `origin` | Provenance, **per facet not per doll**: which photo, conversation, VLM run, upstream URI, or manual entry produced each facet, with confidence per facet. We keep the reasoning VOID throws away, and we keep who said it. |
| `playthrough` | Append-only log of every facet change with timestamps. The only place "current" lives. Everything else is a query against a moment. A doll is its trajectory, not its current snapshot. |

## Containers (also dolls)

* **dollhouse** a scoped collection (a room, a household, an apartment). A place doll that also contains other dolls. Nests arbitrarily: Earth contains nations contains states contains cities contains homes contains people contains cells.
* **playset** a multi-doll event or scenario (the night we met, a move, a news event). A book is a playset: the book doll plus character dolls plus concept dolls plus place dolls, all linked by `company` edges.

## The recursive self

Every facet of a doll can point to a doll that has its own `self`, and so on, until atom dolls (literals with no further unpacking). This is why `kind` is an open string and why a word like "self" can be discombobulated and still usable: the word "self" is a doll with its own `playthrough` showing how its meaning has shifted across philosophy, a life, a conversation. We do not have to define a term before we use it. We give it somewhere to live.

## Scope (federation ready, v1 ships local)

Every facet carries a `scope`:

* `canonical` shared, sourced from upstream (Wikipedia, ISBN, OpenStreetMap, Open Library). Same across every dollbrain that holds a copy.
* `private` personal overlay. Never leaves the owner's machine. **Default.**
* `published` personal overlay deliberately opted into sharing with followers.

Publishing is a deliberate act, per facet, per doll. You can publish your reading notes on a book without publishing your fight about it. v1 is fully local (every facet `private` or `canonical` from upstream pulls). v2 turns on publish and follow. The schema accommodates federation now so it is not locked out later.

## Canonical facets are attested, and may hold contradiction

A `canonical` facet is not a bare value. It is an object:

```
value:        the claim (or a list of competing claims)
asserted_by:  source references (Wikipedia revision, article, witness video, manual)
asserted_at:  when first asserted
entered_at:   when it entered this dollbrain
confidence:   established | contested | disputed | unknown
```

When upstream sources disagree, **both are kept**. No forced reconciliation. The dollhouse holds the disagreement. Reading a canonical facet returns the value and the receipt, so the rendering layer says "as recorded in X on date Y" rather than "X is true." Canonical can age, be backfilled, and be revised; it is never ground truth. Original assertions are never lost when later disputed. There is always a start time: everything before it is reconstructed from sources, attributed, with confidence flagged.

## Two different confidence measures, never conflated

* **Measurement confidence** for tangible facts with a definitive referent (a planet's position, an ISBN, a mass). Real, but decays with time and is timestamped in `playthrough` ("true as measured then").
* **Corroboration plus a divergence registry** for accounts of events. **Never a truth score.** Corroboration measures how many perspectives converge, which is not accuracy. Consensus must never masquerade as truth.

### Corroboration is weighted by independence

Ten people repeating one wire report is one source echoed, not ten corroborations. Once perspectives hear each other they are no longer independent; apparent agreement rises while evidential weight falls. So `origin` provenance is load bearing for telling convergence from contagion. Independent agreement is strong signal. Downstream agreement is near zero. **Divergence held against exposure to the consensus is the loudest signal of all.**

### The outlier is signal, never noise

Divergent accounts are never pruned, downweighted, or averaged. They are preserved at full fidelity, and the fact of divergence becomes its own recorded datum. This is the structural cure for model hallucination (averaging toward the probable and losing the specific true detail): a system forbidden from smoothing cannot smooth away the true odd detail.

## Witness layer on playsets

Every participating doll has its own `playthrough` entry for an event. An event has a canonical layer (time, place, externally documentable facts) and a witness layer (each participant's perspective). They do not have to agree. A query returns the witness slice the querier is entitled to (their own, published slices from people they follow, and the canonical layer). The dollhouse is a graph of testimony, not a single historical thread.

## The four temporal modes

When new information meets existing information there are at least four possible relationships. The relationship between old and new is itself an **explicit, recorded, typed, provenanced edge** (which mode, who or what asserted it, when, why), so it is contestable and reversible. You classify per case, record the classification as data, and revise later because nothing is destroyed.

1. **Evolution (drift)** the thing genuinely changed; both true at their own times; the sequence is the meaning (gender over time, a planet's position). Native to `playthrough` timestamps.
2. **Coexistence (contradiction)** both live simultaneously, different perspectives, neither supersedes (the witness layer).
3. **Supersession (correction)** new makes old no longer usable as true, retained only as "used to be considered this way" (Pluto was a planet). A demotion.
4. **Refinement (accretion)** new adds precision; old stays, new nests under it.

Two bias rules keep the uncertainty safe:

* **The burden of proof is on supersession.** Coexistence is the default because it is the only non-destructive choice. Demoting to history-only requires positive recorded justification. When in doubt, hold both.
* **Even supersession never deletes, only changes status.** Status flips from live to superseded, and that flip is itself a timestamped `playthrough` event. The old fact stays queryable as "believed true X to Y, superseded at Y by Z on basis W." Status is a temporal facet.

When ingestion hits a conflict it cannot confidently classify, the resident agent asks rather than auto-resolving (see [AGENT.md](AGENT.md)).
