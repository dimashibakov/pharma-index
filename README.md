# Pharma Litigation Index

Public consumer web product: search a drug and see its litigation history —
current and completed lawsuits, FDA regulatory notices — plus a transparent,
weighted **Litigation Signal Index**. A public-records aggregator. **Not medical
or legal advice; a lawsuit is an allegation, not proof of harm.**

## What's here

```
index.html        Single-file front-end (vanilla JS, no build step).
                  Hash routing, ⌘K omni-search, SVG gauge. Fetches live
                  from Supabase PostgREST with the publishable (anon) key.
vercel.json       cleanUrls.
assets/           Logo used by the page.
db/schema.sql     Reference schema for the Supabase database (7 tables, RLS).
docs/             Methodology, data sources, architecture decisions.
```

## Data

Lives in a dedicated Supabase project (**not** shared with Zenlo infra).

| | |
|---|---|
| Project ref | `pbzkpsrmsvapyjbdatdr` |
| REST base | `https://pbzkpsrmsvapyjbdatdr.supabase.co/rest/v1/` |
| Key | publishable / anon (read-only; RLS SELECT-only on all tables) |

The page assembles its client model from seven tables: `drugs`,
`signal_components`, `litigation`, `manufacturers`, `drug_manufacturers`,
`litigation_defendants`, `data_sources`. The displayed manufacturer is derived
from `drug_manufacturers.role = 'current_manufacturer'`, so succession renders
correctly (e.g. OxyContin → Knoa Pharma current, Purdue originator).

The anon key is intentionally public: it only grants the read policies in
`db/schema.sql`. All writes happen server-side via the Supabase MCP / dashboard.

## Scoring

The index is **not** "minus points per loss". Over 95% of pharma suits settle
with no admission of liability, and case counts track sales volume more than
danger. It is a weighted composite:

```
Index = Σ ( componentScore × weight )  over components that have data
  regulatory 30% · litigation-volume 25% · AE-severity 25% · court-outcomes 20%
  outcomes pending → that weight is redistributed, confidence lowered
```

The index belongs to the **drug**, not to any manufacturer, and does not transfer
to a successor as its own liability. See `docs/methodology.md`.

## Deploy

Static site on Vercel. Any push to `main` builds production. No env vars needed —
the Supabase URL and publishable key are baked into `index.html` (safe, read-only).

## Status

Prototype. The page carries `noindex` until per-entity canonical URLs replace
hash routing for SEO (see `docs/architecture.md`, ADR-001).
