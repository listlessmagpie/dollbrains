# Governance, Privacy, and Epistemology

This is the layer that decides who can see what, what an institution owes the world, and how the system holds truth without adjudicating it. Much of it is policy that the schema enforces structurally.

## The transparency gradient

Privacy scales down to the individual; transparency scales up to the institution. Entitlement is a function of what kind of doll holds the facet.

* **Human dolls** full privacy. Default `private`. Publishing is a deliberate per-facet act. The absolute floor.
* **Role dolls** (a person acting as editor, agent, CEO) privacy on the human underneath, none on the role's actions. What you did as a private person is yours; what you did wearing institutional authority is public.
* **Institution dolls** (outlet, agency, corporation) zero privacy. An institution cannot hold a `scope: private` facet. This is a type error, not a setting.

The more power and the less personhood, the more transparency required.

## Transparency is not the same as open accessibility

A firehose of total institutional visibility is its own harm: it floods, it exposes the individuals inside institutions, and it can be weaponized. Existence is unconditional; access is conditional. The institutional obligation splits four ways, and only the last is gated:

1. **Must record.** It cannot choose not to log its own conduct.
2. **Cannot tamper.** Append-only, tamper-evident.
3. **Cannot destroy or deny.** The existence of the record is always public, even when contents are not.
4. **Must surrender contents on legitimate cause.** No stalling, bad-faith redaction, or refusal.

**Mechanism: commitments.** A published hash proves a record exists, is sealed, and was timestamped, without revealing contents. Existence, timestamp, and integrity are universally and permanently public; contents stay closed until legitimate cause unlocks them. A later doctored version cannot match the up-front commitment, so tampering and deletion are impossible by construction.

**The hard part is governance, not crypto: who decides what counts as legitimate cause.** The gate cannot be held by the subject of the transparency, or it is hollow. The arbiter must be neutral (a court, an ombudsman, a random jury, a cooperative body), depending on domain. Crypto enforces the seal and proves integrity but cannot decide whose hand turns the key. That decision stays human. (This is the cooperative-governance question that the Whimsible thread has to answer.)

## Authored versus observed dolls

A doll is a perspective on a thing, not the thing's self-description. This resolves the non-participant problem (the US government does not want to be a doll, and does not get a choice).

* **Authored dolls** (participating humans) self-hosted, own their private layer, choose what to publish. Consent-based.
* **Observed dolls** (non-participants, institutions, the dead, the fictional, the past) hosted by whoever observes them, built only from public record, no private layer, no consent required because no privacy was ever owed.

The transparency gradient is the firewall: an observed doll can only be built from already-public facts. A private human cannot be made an observed doll, because their facts are not in the public layer. An institution cannot escape becoming one, because all its facts are.

### Observed dolls need no participation, and refusal is not a violation

There is nothing to compel, because there is no rule. An observed doll exists from the first observer, built from the aggregate of everyone who records their lived experience of the thing. The bakery does not create its own doll; it exists the moment one user walks in. The subject can later **occupy** its doll (add attested testimony to the witness layer) but can never create, control, or delete it. It was never theirs; it is the commons' model of them.

* What gets recorded is not "the thing itself" but the texture of how people relate to it (activities, reasons, feelings, what they get, when they go). That is the observer's own life, theirs to record.
* **Opacity is a signal, not a violation.** An institution that obscures itself emits a fact about itself, recorded by everyone who notices. Enforcement is emergent: people read the signal and choose. No compliance body needed (compliance regimes get captured; signals do not).
* **Absence is ambiguous, so distinguish quality.** Active refusal (someone asked, it stonewalled, a recorded interaction with content) is real data. Mere thinness (not observed yet, an empty page) is no signal. Only refusal carries weight.

## Privacy travels with the subject, not the holder

A facet's scope is set by who it is **about**, not who holds it. Your interaction at the bakery is your facet even though the bakery witnessed it, so the bakery cannot publish it identifiably. This corrects a naive reading of "institutions get zero privacy": institutions are transparent about their own conduct, never about the private humans they touch.

### The mosaic problem and its honest limit

How plus when plus where plus frequency re-identifies a person even with no name attached. De-naming an event is not enough; a single de-named event is a fingerprint. Rules:

* Institutions publish **aggregates over enough people** (k-anonymity), never per-event records. "Served about 200 people this week," not "a customer came in."
* The two witness entries of an event do not link in public. The join key (who) exists only on the subject's own device.
* **Honest limit:** linkage cannot be made impossible, only expensive and bounded. Differential privacy (measured noise plus a privacy budget) is the only framework that gives a mathematical bound, and it works by capping released truth. Plus aggressive data minimization (publish the least that satisfies the actual transparency duty, almost always a count).

### Your observation can sweep up other private humans

Recording your own experience is airtight, but it can incidentally contain other private people ("I saw my ex there"). The firewall is on what you can **broadcast**, not what you can **know**. Your private layer, on your own machine, may hold anything you perceived, names and all. The brakes apply only at publishing: scrub the incidentally-caught private humans, same aggregate and minimization rules.

Privacy here is not a wall around the dollbrain. It is a property that clings to a subject's facets and follows them into the world, constraining what anyone may say about them regardless of where they were encountered.

## The epistemology: fact versus account

There is no historical fact, only historical account, true even of events that just happened. Accuracy requires a definitive referent that mostly does not exist. Objective versus subjective is a gradient, not a partition; the only near-objective things are things that cannot change themselves, and even those drift by time, use, and wear. So everything is timestamped, including tangible facts. (See [SCHEMA.md](SCHEMA.md) for the two confidence measures, independence weighting, and the outlier-as-signal law.)

### Reality as agreed convergence

Competing frames ("software is transcribing me" versus "I am doing magic") are account facets, or concept dolls, over a single referent event, differing in language and frame, neither demoted. The system holds all and privileges none, because the divergence-preservation law forbids picking. Reality being agreed-upon makes it a **convergence**, not a deletion of the divergent. To the person inside the magic frame it is not less true.

## The line the system must not cross

The purpose is to archive human experience faithfully enough that a person can practice **discernment** on it. The system presents the spread, the convergence structure, the divergences, and the independence weighting, then stops. It never picks a winner, never ranks accounts by the moral worth of who gave them, and never decides which version is true. The system guarantees the record is complete and honestly structured (nothing silently deleted, no false consensus manufactured). The reader decides what they find credible and agree with. This is sovereignty applied to truth itself.
