# dolls/

Worked example dolls. These are **hand-authored illustrations of the schema**, not engine output. There is no engine yet (see [../ROADMAP.md](../ROADMAP.md), Phase 2).

* `sun.example.json` the Sun as an astronomical body doll. Shows canonical timed facts, computed aspect edges, per-facet provenance, an empty private `story` layer, and how an ephemeris reading becomes a `playthrough` entry.

When the engine exists, real dolls will be content-addressed (the `id` will be a hash of the canonical facets) and most facets will be authored by ingestion rather than by hand. The examples exist to make the schema concrete and to argue against before code locks anything in.
