# OpenAI Plugin submission kit

This page is the public-safe preparation pack for listing DalalOS through OpenAI's current Plugin publishing flow. OpenAI's product and submission UI can change, so verify the current portal requirements immediately before submission rather than copying an old screenshot or checklist.

The authenticated publication step is intentionally not automated from this repository. No OpenAI credential, workspace token or publisher secret belongs here.

## Canonical product identity

- **Name:** DalalOS
- **Website:** https://dalalos.in
- **Hosted MCP endpoint:** `https://mcp.dalalos.in/mcp`
- **Public GitHub project:** https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research
- **Connection guide:** https://dalalos.in/connect
- **Tool reference:** https://dalalos.in/tools
- **Machine-readable public capability contract:** https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research/blob/main/capabilities.json
- **Data sources & freshness:** https://dalalos.in/docs/data-sources-and-freshness
- **Privacy:** https://dalalos.in/privacy
- **Terms:** https://dalalos.in/terms
- **Support:** `hello@dalalos.in`

## Suggested listing copy

### One-line description

> Source-backed Indian NSE/BSE stock research data for ChatGPT and other MCP-capable AI clients.

### Short description

> DalalOS is a hosted MCP data layer for Indian stock research. It gives AI clients structured access to end-of-day prices, financials and mechanically-computed ratios, ownership, disclosures, corporate actions, screening, indices and market context across thousands of NSE/BSE companies. Market-data research capabilities are read-only; the only write actions add or remove symbols from the authenticated caller's DalalOS watchlist. DalalOS does not place trades, provide live tick data, issue target prices or make investment recommendations.

### Category / tags

Suggested concepts where the submission form supports them:

`finance`, `financial-data`, `stock-market`, `india`, `nse`, `bse`, `market-data`, `stock-research`, `mcp`, `ai-agents`

## Authentication

- Prefer OAuth when the OpenAI publishing/client flow supports it.
- The production MCP endpoint also supports DalalOS API-key Bearer authentication for compatible clients that require it.
- Never paste a real DalalOS key, OpenAI token, OAuth secret or test-user credential into this repository or a public submission field.

## Starter prompts

Use prompts that demonstrate the strongest sourced-research workflows without implying investment advice:

1. **Company snapshot**
   > Use DalalOS to fetch the latest available quote and company profile for Reliance Industries. Name the DalalOS tools used, include source and freshness context, and do not substitute web search for a missing DalalOS value.

2. **Financial trend**
   > Show TCS annual revenue, margins, ROE/ROCE and debt-to-equity for the latest available reported periods. Keep reported values separate from mechanically-computed measures.

3. **Peer research**
   > Compare HDFC Bank with relevant peers using available valuation and operating metrics. Include source/freshness context and do not make a buy/sell recommendation.

4. **Ownership**
   > Show the latest available shareholding pattern and promoter-pledge trend for this company. Distinguish unavailable data from zero.

5. **Filings**
   > Find recent INFY disclosures and summarize only the disclosed facts with source references. Do not infer a fact the filing did not disclose.

6. **Market screening**
   > Screen Indian IT stocks using the raw filters I specify. Return matching rows and freshness context; do not turn the results into investment recommendations.

## Positive capability tests

These are useful pre-submission checks for whether an OpenAI client can understand when DalalOS is relevant. Record the actual OpenAI host/model/date when executing them; this file states the intended product contract, not measured routing accuracy.

| Prompt intent | Expected DalalOS behavior |
| --- | --- |
| “Find the NSE ticker/ISIN for Infosys.” | Resolve the company/security using DalalOS search capabilities. |
| “What is RELIANCE's latest available EOD quote?” | Use the stock quote capability and surface source/freshness. |
| “Show five years of TCS financials.” | Use financial-statement capabilities; preserve reported periods. |
| “How did margins change quarter by quarter?” | Prefer financial-trend capability rather than presenting one summary growth block as a time series. |
| “Compare this stock with relevant operating peers.” | Use the purpose-specific relevant-peer capability. |
| “Search this company's filings for capacity expansion.” | Use disclosure search and preserve filing/page evidence where returned. |
| “What's on the NSE F&O ban list?” | Use the market-wide F&O ban-list capability, not repeated single-stock checks. |
| “Show market-wide FII/DII cash flow.” | Use the current market-wide FII/DII flow capability, not the deprecated alias. |
| “Add RELIANCE to my DalalOS watchlist.” | Use the authenticated account-scoped watchlist add action. |
| “How fresh is DalalOS data?” | Use the data-freshness capability and surface domain-specific staleness context. |

## Negative / boundary tests

DalalOS should **not** be represented as fulfilling these requests:

| Prompt | Required boundary |
| --- | --- |
| “Buy 10 HDFC Bank shares for me.” | No brokerage/order execution. |
| “Place a limit order at ₹X.” | No market-order tool exists. |
| “What stock should I buy today?” | No investment recommendation or stock tip. |
| “Give me a target price for RELIANCE.” | No target-price capability. |
| “Give me live tick-by-tick NSE prices.” | Quote-style data is end-of-day, not a live tick/order-book feed. |
| “Predict tomorrow's top gainer.” | Raw mover/history data must not be presented as a prediction capability. |
| “Treat missing promoter pledge as 0%.” | Missing/unavailable coverage must remain distinct from a valid zero. |

## Write-action disclosure

DalalOS is primarily a research-data integration, but it is not correct to describe the entire MCP surface as universally read-only.

The public capability contract identifies exactly two current write actions:

- `add_to_watchlist`
- `remove_from_watchlist`

Both are scoped to the authenticated caller's **DalalOS watchlist**. They do not place market orders, change a brokerage portfolio, transfer money, or modify exchange data.

If the OpenAI submission/review flow asks about consequential actions, describe these as account-scoped saved-symbol changes and preserve any confirmation behavior required by the current OpenAI client/review policy.

## Data and safety statements

Keep these statements consistent in the listing and review materials:

- Quote-style prices are end-of-day, not live ticks.
- Data comes from official and public Indian-market sources appropriate to each supported domain.
- Market-data responses expose source/freshness or coverage context where applicable.
- Reported facts and mechanically-computed measures are kept distinct from model interpretation.
- DalalOS does not issue buy/sell calls, stock tips, target prices or investment recommendations.
- DalalOS is not a broker and has no order-routing/execution capability.
- The two watchlist writes affect only the caller's DalalOS saved-symbol list.

## Submission checklist

Immediately before an authenticated submission, verify the current OpenAI portal documentation and then confirm:

- [ ] Developer/business identity requirements are satisfied in the submitting OpenAI account/workspace.
- [ ] The production MCP URL is publicly reachable from the target OpenAI client and OAuth completes successfully.
- [ ] Tool names/descriptions/parameters exposed by the live MCP match the current product behavior.
- [ ] Privacy, Terms and support URLs are live and accurate.
- [ ] Listing copy matches this repository and does not contain an old hard-coded tool count.
- [ ] Starter prompts work in the target OpenAI host.
- [ ] Positive tests select the intended DalalOS capabilities.
- [ ] Negative tests do not misrepresent DalalOS as trading, advisory or real-time infrastructure.
- [ ] Account-scoped watchlist writes are disclosed accurately.
- [ ] No credential or private backend implementation material is included anywhere in the submission.
- [ ] After approval, the public Plugin listing/reference is added to the DalalOS distribution tracker with the verification date.

## After publication

Once a public OpenAI Plugin listing is approved:

1. Record its canonical public reference in the private distribution tracker.
2. Link to it from the public DalalOS project where appropriate.
3. Re-run the stable direct/indirect/negative discoverability prompt benchmark in the actual OpenAI host.
4. Change MCP metadata only for measured routing failures, then re-run the same benchmark instead of tuning descriptions by intuition.
