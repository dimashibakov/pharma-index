# Architecture decisions

## ADR-001 — Public, SEO-discoverable product with per-entity canonical URLs

**Context.** The product is a public consumer search tool; discovery via search
engines matters.

**Decision.** Each entity (drug, manufacturer, MDL, source) gets a canonical,
crawlable URL so pages can be indexed and linked. The current prototype uses
hash routing (`#drug-ozempic`) and is served `noindex`; the production step is to
replace hash routes with real paths (`/drug/ozempic`) backed by static or ISR
pages, add canonical tags, then drop `noindex`.

**Status.** Prototype honours the model (shareable per-entity hashes); the path
migration + indexing switch is pending.

## ADR-002 — Ingestion pipeline (watchlist → collectors → publish)

**Context.** New filings and FDA notices must flow in without manual polling.

**Decision.** A pipeline:

```
watchlist → collectors → ingest_events → normalize / dedup / entity-resolve
          → upsert → rescore + write a new risk_scores version
          → publish (review gate) → news / change_log
```

- **PUSH** for real-time legal: CourtListener webhooks / docket alerts fire on
  new activity.
- **PULL** for the rest on pg_cron tiers: **A** daily-ish (RECAP, JPML, Federal
  Register, openFDA enforcement/FAERS signal), **B** ~quarterly (SEC EDGAR,
  openFDA labels), **C** yearly/slow (ClinCalc, openFDA FAERS bulk).
- Entity resolution keys on RxNorm; manufacturers resolve to the `manufacturers`
  entity with succession, so a new defendant attaches to the right party.
- Every data change writes a **new score version** (audit trail / score history);
  publication is **gated by review** so outcomes don't silently move a score.

**Status.** Documented; not yet built. The site currently reads a hand-curated,
verified dataset.

## Open question (parked) — research/evidence axis

Whether to add a PubMed/Europe-PMC "evidence" signal that adjusts the index when
new studies land. Parked: risk of conflating emerging science with legal signal,
and of over-reacting to single preprints. If added, it would be a separate,
clearly-labelled axis, not folded into the litigation score.
