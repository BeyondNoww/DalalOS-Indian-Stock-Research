# Ask AI about Indian earnings calls and filings, with the page behind each answer

How to search what an Indian company's management said on earnings calls (concalls) from Claude or ChatGPT with the DalalOS MCP server, and see the filing page behind each passage.

Uploading one transcript PDF to an assistant works for one call. This covers the other case: you want to search across a company's filed calls and check the source.

## Connect

Add `https://mcp.dalalos.in/mcp` as a connector and sign in once. Steps for each client are in the [README](../../README.md#connect). The free tier needs no card.

## Search the calls

`search_company_disclosures` searches a company's already indexed filings and returns matching passages with the filing date, the document type, the original page number and the attachment link. Pass `concall` in `document_types` to search calls only. The assistant first resolves a company name with `search_stocks`.

> Search the indexed concall filings of LT for 'order book' and 'demand outlook'. Show each passage with the filing date and the source page, and tell me how much of the filings was covered.

> What did LT management say about capacity expansion on recent calls? Quote the passages with page numbers and give the filing links.

Name the topic in a few distinctive words. A passage matches if it contains any of them, best matches first. AND and OR are not operators.

## Read the facts in one filing

`get_stock_events` lists a company's filings and gives each an id. `get_filing_extract` takes that id and returns the extracted facts, each with a source quote and a page. A filing that has not been read yet is queued and can take a few minutes. It counts against a small daily allowance. Reading a filing that is already extracted does not.

Financial-results filings are not extracted as facts. Use `get_financials` for the figures.

## Put what management said next to what was reported

DalalOS has no tool that scores guidance against results. You can line the two up yourself:

> For LT, find what management said about revenue growth or order inflow guidance on a recent call. Then get the reported revenue for the same period with get_financials. Show the two side by side, with sources, and do not say whether they delivered.

- Guidance is often a range or a qualitative phrase. Ask for the exact words before comparing.
- Use the same period, basis (standalone or consolidated) and unit on both sides.
- The filed results are the primary record. Open them for any figure you rely on.

## Limits

- The search covers indexed filings only and never fetches or queues a new one. No match does not mean the company never said it. Ask the assistant to show the coverage.
- An AI summary can drop or bend a detail. Ask for the quoted passage and the page.
- DalalOS returns passages and figures, with no buy or sell calls, ratings or target prices.

Page-level checks are on the website: https://dalalos.in/use-cases/read-bse-nse-company-filings-mcp.

This is research data, not investment advice.
