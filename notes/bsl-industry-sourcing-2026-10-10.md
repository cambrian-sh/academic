# BSL adoption evidence — industry-only sourcing plan (2026-10-10)

For the ADR-0057 open-core paper (§5 argues, never cites peer-reviewed
BSL literature — none exists). This note converts "BSL adoption rates
(industry data only)" from open debt into a fetch checklist. Nothing
below is citable yet except the one marked previously-verified.

## Established (prior sessions)

- No peer-reviewed BSL/open-core literature exists (systematic + ICLR
  passes). No aggregate BSL adoption *rate* found anywhere — treat any
  quoted percentage as vendor marketing unless traced to a dataset.
- Redis 2024-03-20 move was RSALv2+SSPLv1, NOT BSL (previously verified;
  do not cite Redis as a BSL datapoint).

## Candidate datapoints (all UNVERIFIED — fetch before citing)

| # | Claim to check | Where to verify |
|---|---|---|
| 1 | MongoDB SSPL switch (Oct 2018) + OSI rejection of SSPL | MongoDB blog 2018-10-16; OSI license-review archive |
| 2 | Elastic license change (2021, SSPL/Elastic License) and AGPL return (Aug 2024) | Elastic blog 2021-01-14 and 2024-08-29 |
| 3 | HashiCorp BSL 1.1 adoption (Aug 2023, Terraform/Vault/etc.) | HashiCorp blog 2023-08-10 |
| 4 | Sentry BSL (2019, with $2M self-host carve-out figures) | Sentry blog 2019-11-14 |
| 5 | CockroachDB BSL (2019) then BUSL/free-tier changes | Cockroach Labs blog 2019-06-04 |
| 6 | MariaDB MaxScale BSL (2016 — earliest major BSL use) | MariaDB blog 2016 |
| 7 | Any survey with a BSL share number (OpenLogic State of OSS, FOSSA, Tidelift) | latest editions; expect zero BSL-specific numbers — record absence as finding |

## What the paper may honestly claim (once fetched)

- Existence + direction of the trend (named vendors moving
  proprietary-adjacent), NOT a rate.
- RSAL/SSPL/BSL distinctions kept exact (Redis lesson).
- Every number labeled industry/vendor-sourced in-text, never in a
  peer-reviewed citation slot.
