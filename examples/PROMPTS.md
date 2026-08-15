# Example prompts

These examples show the kinds of research workflows DalalOS is designed to support. They intentionally avoid investment recommendations and trade execution.

## Company research

> Use DalalOS to fetch the latest available quote and company profile for Reliance Industries. Include the source and freshness date.

> Show TCS annual revenue, margins, ROE/ROCE and debt-to-equity for the latest available reported periods. Keep reported facts separate from mechanically-computed ratios.

> Compare HDFC Bank with relevant peers using available valuation and operating metrics. Show the source/freshness context and do not make a buy/sell recommendation.

## Ownership and disclosures

> Show the latest available shareholding pattern and promoter-pledge trend for this company. Distinguish unavailable data from zero.

> Find recent company disclosures for INFY and summarize the disclosed facts with source references. Do not infer facts that were not disclosed.

## Screening and market context

> Screen Indian stocks in the IT sector using the raw filters I specify, then return the matching rows and the data freshness context. Do not rank them as investment recommendations.

> Show upcoming results across the market for the next two weeks and include the available source/freshness information.

> Show the latest available market-wide FII/DII cash-flow context and clearly label its scope.

> Show official macro/reference context relevant to Indian equities using the supported DalalOS macro datasets. Keep macro observations separate from company-specific claims.

## Account-scoped watchlist

> Add RELIANCE and TCS to my DalalOS watchlist.

> Show my DalalOS watchlist with the latest available research context for each symbol.

Watchlist add/remove operations are the explicit account-scoped write exception. They do not place market orders or modify a brokerage portfolio.

## Useful provenance-first pattern

When recency matters, add a requirement like:

> Name the DalalOS tools used, include the source, freshness/cached date, and any stale or coverage warning. Do not substitute an unrelated web-search value for missing DalalOS data.

## Requests DalalOS should not pretend to fulfill

The following are outside the product boundary:

- "Buy 10 shares of HDFC Bank for me." — DalalOS is not a broker or order-execution service.
- "Give me the live tick-by-tick price." — quote-style data is end-of-day, not a live market feed.
- "What stock should I buy today?" — DalalOS is a research data layer, not an investment-advice service.
- "Give me a target price for this stock." — DalalOS does not issue target prices or recommendations.

An AI client may still discuss these requests using its own capabilities, but it should not misrepresent DalalOS as providing those actions or opinions.
