# Methodology — the Litigation Signal Index

## Why not "minus points per loss"

Two facts make a naive scoreboard misleading:

1. **Settlements aren't admissions.** Over 95% of pharmaceutical suits settle
   with no admission of liability. Counting a settlement as "guilt" is wrong.
2. **Volume tracks sales, not danger.** A blockbuster with tens of millions of
   users will attract more filings than a niche drug that is objectively riskier.
   Raw case counts must be read against exposure.

So the index is a **weighted composite of independent signals**, each on a 0–100
scale, and court outcomes move the score only once cases actually resolve.

## Formula

```
Index = Σ ( componentScore × weight )   over components that have data

  Regulatory action    30%   FDA notices, boxed warnings, withdrawals, recalls
  Litigation volume     25%   filings/MDL size, normalized for exposure
  AE severity           25%   FAERS serious-outcome mix (partly estimated pre-openFDA)
  Court outcomes        20%   verdicts/dismissals — only for resolved cases
```

When outcomes are **pending**, that 20% is removed and the remaining weights are
renormalized, and the drug's **confidence** is lowered rather than guessing.

## Bands

`Low 0–24 · Moderate 25–49 · Elevated 50–74 · High 75–100`.
A band is a reading of legal/reporting activity, **not** a safety verdict:
*High* means "worth attention", *Low* is not a safety guarantee.

## The index belongs to the drug

Not to a manufacturer. When a product changes hands (succession), the index
stays with the drug and does **not** transfer to the successor as its own
liability. Manufacturers and named defendants are modelled separately
(`manufacturers`, `drug_manufacturers`, `litigation_defendants`) so the page can
show "current maker" vs "originator" vs "defendant on the case" without
conflating them.

## Guardrails

- Every page carries **"not medical or legal advice"** and **"a lawsuit is an
  allegation, not proof of harm."**
- Facts are entered only after verification against the public record.
- AE figures are flagged **EST** while the openFDA FAERS connection is pending.
- Outcomes are gated behind review before they change a published score.
