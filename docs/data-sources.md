# Data sources

Legal and medical public records behind the index. Access: **available** (free
API/bulk), **limited** (aggregates only), **paid**, **unavailable** (no
machine-readable feed). Update tier drives the pull cadence (ADR-002):
A = days, B = ~quarterly, C = yearly/slow.

## Legal

| Source | Access | Tier | Freshness | Notes |
|---|---|---|---|---|
| CourtListener / RECAP | available | A | Days | Free API — main source for automation |
| JPML pending reports | available | A | ~30 days | MDL pending counts; issued on the 1st monthly |
| SEC EDGAR | available | B | Quarterly | Litigation disclosures — public companies only |
| PACER | paid | — | Realtime | $0.10/page, no bulk API — verification only |
| State courts (50 states) | unavailable | — | Varies | No unified machine-readable DB — biggest blind spot |
| Europe national courts | unavailable | — | Varies | Fragmented, multilingual, GDPR limits |

## Medical / regulatory

| Source | Access | Tier | Freshness | Notes |
|---|---|---|---|---|
| openFDA — enforcement | available | A | Weekly | Recalls Class I/II/III |
| FDA FAERS Quarterly Signal reports | available | A | Quarterly | Auto-seed for the signal trip-wire |
| Federal Register | available | A | Day of publication | FDA notices, withdrawals, safety communications |
| openFDA — labels | available | B | On label update | Labels, boxed warnings, contraindications |
| openFDA — FAERS | available | C | 1–2 quarters | Free; 1k req/day no key, 120k with key |
| ClinCalc DrugStats | available | C | Yearly (MEPS) | Top drugs by prescriptions (demand/exposure seed) |
| RxNorm (NIH) | available | — | — | Canonical key for dedup / entity resolution |
| EMA EudraVigilance (adrreports.eu) | limited | — | — | Aggregates only, no raw data |
| IQVIA | paid | — | — | Expensive; ClinCalc substitutes for the MVP |

The canonical copy of this table lives in the `data_sources` table and in Notion;
this file is a snapshot for the repo.
