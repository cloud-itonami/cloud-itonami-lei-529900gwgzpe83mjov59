# cloud-itonami-lei-529900gwgzpe83mjov59

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by Manchester United plc.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**Manchester United plc**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: Manchester United plc
- **LEI (ISO 17442)**: [529900GWGZPE83MJOV59](https://search.gleif.org/#/record/529900GWGZPE83MJOV59) (GLEIF-verified)
- **Jurisdiction**: KY
- **Website**: https://www.manutd.com
- **Ticker**: MANU (NYSE)

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 10 verified registry facts with per-fact provenance (9 about the
  entity itself, 1 security). **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind
them. `facts.edn` now carries them as data, and every value in it was read out of
a public registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T03:08Z, golden copy 2026-08-22T16:00Z) — the LEI record (legal name
`Manchester United plc`, entity **ACTIVE**, registration **LAPSED**: the
renewal fell due 2020-01-03 and the record was last updated 2021-07-16, with
GLEIF's conformity flag `NON_CONFORMING`; entity status and registration
status are different fields and are recorded separately, and an ACTIVE company
can hold a LAPSED LEI), its 1 ISIN (`KYG5784H1065`, counted from
`meta.pagination.total` of a single page whose `lastPage` is 1, so it is also
mirrored as its own `:security` entity), its managing LOU and LEI-issuer
accreditation (Herausgebergemeinschaft Wertpapier-Mitteilungen Keppler,
Lehmann GmbH & Co. KG, marketed as WM Datenservice, LEI
`5299000J2N45DDNE4Y28`, accredited 2017-04-13), registration authority
`RA000086` (General Registry, Cayman Islands, registration number `268512`),
ISO 20275 legal form `8888` (the catch-all code; the registry's name list for
it is empty, which is recorded as-is rather than filled in), reporting
exceptions at both consolidation levels (`NO_KNOWN_PERSON` — GLEIF records no
parent, direct or ultimate, for this entity), and **0 direct children**, read
from `meta.pagination.total` of a 15-per-page request — a measured zero, not
an unasked question. The `direct-parent` and `ultimate-parent` endpoints
answered `404` because GLEIF publishes the exception side of that pair for this
entity, which the checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the
live sources, `1` a citation broke or a fact drifted, `3` the check could not be
performed at all — an absent `facts.edn`, or every request failing at the
transport level. A check that could not run must not be indistinguishable from a
check that ran and found nothing, so it refuses to report a pass rather than
exiting 0. All four outcomes were exercised before this landed: unmodified `0`;
`:registration/next-renewal-date` edited one year forward → `1` naming
`DRIFT gleif-lei-record :registration/next-renewal-date`; the `gleif-isins`
entity deleted → `1` naming `ADDED gleif-isins`; the GLEIF host in the checker
rewritten to an unresolvable name → `3` (`INCONCLUSIVE … refusing to report a
pass`). Rewriting the host only inside `facts.edn` is *not* the transport case:
the live URLs are derived from `blueprint.edn`, so that edit reports as
`DRIFT … :source/url` findings (`1`) — a recorded citation that no longer names
its source is drift, not an outage.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
