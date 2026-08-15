# Data sources and freshness

DalalOS is designed for source-backed Indian stock research. This public document describes the provenance and freshness contract a user or AI client should rely on; it intentionally does not document the service's internal ingestion, storage, deployment, or processing architecture.

The maintained canonical reference is:

https://dalalos.in/docs/data-sources-and-freshness

## Source families

DalalOS uses official and public sources appropriate to each supported data domain. These include:

| Source family | Used for |
| --- | --- |
| NSE official archives and exchange-published datasets | End-of-day market data, index/exchange datasets and other supported NSE-published facts |
| BSE official interfaces and exchange filings | Security/company data, disclosures, corporate actions and filing-driven datasets |
| SEBI-mandated BSE-XBRL filings | Reported company financial statements and filing-derived line items used for mechanically-computed ratios |
| Named official public-sector publishers | Supported macroeconomic and reference datasets, such as those published by MOSPI, PPAC or FBIL |

The exact source attached to a result matters more than a generic provider label. When a DalalOS response includes provenance metadata, the client should surface it rather than restating a number without its source context.

## Freshness is domain-specific

DalalOS is **not a real-time market feed**.

- Quote-style stock-market data is end-of-day.
- Financial statements update when the relevant reported filings are available to the supported data surface.
- Corporate actions, results, ownership and disclosures follow their own publication/reporting cadence.
- Macro/reference datasets follow the publication cadence of their official publisher.

Do not assume every field in a research answer shares one timestamp.

## Freshness metadata

Depending on the tool and data domain, DalalOS responses may expose fields or notes such as:

- `source` — provenance for the returned data;
- `cached_at` or an equivalent freshness timestamp — when the relevant result was refreshed or assembled;
- `stale` — whether the data has exceeded the service's freshness policy for that domain;
- coverage notes or reason codes — whether a value is unavailable, not applicable, incomplete, or otherwise qualified.

When freshness itself is the question, connected clients can use DalalOS's data-freshness capability and should surface any stale/coverage warning instead of silently omitting it.

## Missing or unavailable data

DalalOS does not replace an unavailable disclosed fact with an invented value. A result may explicitly indicate missing coverage, a stale state, a not-found condition, or another domain-specific qualification.

A valid zero and missing data are not the same thing; clients should preserve the distinction returned by DalalOS.

## Research boundary

Source-backed does not mean investment advice. DalalOS provides reported facts and mechanically-computed measures for research workflows. It does not provide buy/sell recommendations, target prices, brokerage execution, or live tick/order-book data.
