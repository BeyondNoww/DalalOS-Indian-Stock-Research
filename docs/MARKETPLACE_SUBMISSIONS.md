# Marketplace submission kit

Public-safe preparation pack for listing DalalOS in AI-client marketplaces beyond the OpenAI plugin (see [`OPENAI_PLUGIN_SUBMISSION.md`](OPENAI_PLUGIN_SUBMISSION.md)) and the generic directory kit ([`DIRECTORY_LISTING.md`](DIRECTORY_LISTING.md)). Vendor portals change; check each vendor's current requirements immediately before submitting.

The authenticated submission steps are done by a DalalOS owner in each vendor's portal. No credential, test-account password or publisher token belongs in this repository.

## Status

| Marketplace | Route | State |
| --- | --- | --- |
| Official MCP Registry | `mcp-publisher` + domain proof | Listed as `in.dalalos/mcp` |
| ChatGPT | OpenAI plugin | Published (India) |
| Glama | `glama.json` in this repo | Listed |
| Claude Connectors Directory | Developer portal | See [Claude](#claude-connectors-directory) |
| Claude Code plugin | `.claude-plugin/` in this repo | Manifests in repo; plugin and marketplace added with this change |
| Gemini CLI extension gallery | `gemini-extension.json` + tagged release | Manifest in repo; needs the topic and a `v1.0.0` release |
| Google Antigravity MCP Store | Forum listing request | See [Antigravity](#google-antigravity-mcp-store) |
| Cursor | Marketplace form or cursor.directory | See [Cursor](#cursor) |
| Smithery | `smithery.yaml` + `smithery.ai/new` | Config in repo; confirm the listing is live |

## Identity (reuse everywhere)

- **Name:** DalalOS
- **Endpoint (streamable HTTP):** `https://mcp.dalalos.in/mcp`
- **Registry name:** `in.dalalos/mcp`
- **Website / docs:** https://dalalos.in , https://dalalos.in/connect , https://dalalos.in/tools
- **Privacy / terms:** https://dalalos.in/privacy , https://dalalos.in/terms
- **Support:** `hello@dalalos.in`
- **Authentication:** OAuth 2.0 (PKCE S256; dynamic client registration and client-ID metadata documents supported). Unauthenticated tool calls return `401` with a `WWW-Authenticate: Bearer resource_metadata=...` header.
- **Tool count:** read `preferred_tool_count` from [`../capabilities.json`](../capabilities.json); do not copy a number from prose.

## Claude Connectors Directory

Submit at https://claude.ai/directory/manage → **Submit new** → **MCP connector**. Any paid Claude plan can submit; Team and Enterprise need an Owner (or a role with the Directory permission). Requirements: https://claude.com/docs/connectors/building/submission and https://claude.com/docs/connectors/building/review-criteria.

### Pre-submission checks

- [x] Remote server over HTTPS.
- [x] Every tool carries a human `title` and a `readOnlyHint` / `destructiveHint` annotation. Tools that queue filing extraction or edit the watchlist are not marked read-only; `remove_from_watchlist` is marked destructive.
- [x] Tool names are under 64 characters.
- [x] OAuth discovery: `401` with `resource_metadata`; protected-resource `resource` equals the MCP URL; authorization server supports PKCE `S256`, DCR and CIMD.
- [x] Privacy policy and terms are public.
- [ ] Run every tool through MCP Inspector or as a custom connector and tick the portal's confirmation (owner).
- [ ] Create a **test account with sample data** (a populated watchlist and a few filing extracts) and enter its credentials in the portal's Test step only. Never put them here.
- [ ] Confirm the callback `https://claude.ai/api/mcp/auth_callback` completes sign-in. Claude Code uses a loopback callback on an ephemeral port, which the client-ID metadata document covers.
- [ ] Allow Claude's egress range `160.79.104.0/21` if any rate limiting or allowlisting sits in front of `mcp.dalalos.in`.
- [ ] Upload an icon and pick a permanent URL slug.

### Listing copy

**Name (max 100):**

> DalalOS: Indian Stock Market Research (NSE/BSE)

**One-liner (max 200):**

> Sourced Indian NSE/BSE stock data in Claude: financials, ratios, shareholding, filings, screens and FII/DII flows, each with source and date.

**Description (max 2,000):**

> DalalOS connects Claude to sourced Indian stock-market (NSE/BSE) research data. Ask about a company and Claude pulls end-of-day prices, reported financials and mechanically-computed ratios, shareholding and promoter pledge, corporate actions and dividends, exchange disclosures and filing extracts, peer comparisons, IPOs, index and sector context, FII/DII flows, F&O positioning and macro series.
>
> Every response carries its source and a freshness date, and a value that is not available is reported as unavailable rather than guessed. Quote-style data is end-of-day, not a live tick feed.
>
> DalalOS returns reported facts and mechanically-computed measures only. It does not issue buy, sell or hold calls, target prices or valuation verdicts, and it is not a SEBI-registered investment advisor. It cannot place trades.
>
> Market-data tools are read-only. The exceptions are the filing tools, which can queue background extraction, and the add and remove actions on the signed-in user's own DalalOS watchlist.
>
> Sign in once with Google or an emailed code. The free tier needs no card.

**Categories:** Finance, Research, Data & Analytics

**Use cases (portal Use cases step):**

1. *Company snapshot.* "Fetch the latest quote and company profile for Reliance Industries. Name the DalalOS tools used and include source and freshness."
2. *Financial trend.* "Show TCS annual revenue, margins, ROE/ROCE and debt-to-equity for the latest reported periods. Keep reported values separate from computed ones."
3. *Peer research.* "Compare HDFC Bank with relevant peers on the available valuation and operating metrics."
4. *Ownership.* "Show the latest shareholding pattern and promoter-pledge trend for this company."
5. *Screening.* "Screen Indian IT stocks with the raw filters I specify and return the matching rows."

**Data handling answers:** draw only from the live policy at https://dalalos.in/privacy (last updated 27 October 2026), which states that DalalOS stores the WorkOS user ID and email, the arguments each tool is called with, watchlist symbols, API-key metadata, usage counters and any feedback report the user chooses to send, and that the tools do not read chat history, assistant memory or files. Copy retention, sharing and analytics-provider answers from the policy's own sections rather than paraphrasing them, and re-check them against the page on the day you submit. The policy notes it has not yet had outside legal review; consider that before a public directory listing.

**Compliance step:** the portal asks about financial transactions. DalalOS does not move money, trade or execute orders.

Escalations: `mcp-review@anthropic.com`.

## Claude Code plugin

This repository is also a Claude Code plugin marketplace (`.claude-plugin/marketplace.json`) containing one plugin (`.claude-plugin/plugin.json`, `.mcp.json`). Install:

```bash
/plugin marketplace add BeyondNoww/DalalOS-Indian-Stock-Research
/plugin install dalalos@dalalos
```

Sign in from `/mcp` on first use. Check locally with `claude plugin validate .` before each release; bump `version` in `plugin.json` when the plugin changes.

To list it in Anthropic's official plugin directory, submit this repository through the same developer portal and choose the plugin option.

## Gemini CLI extension gallery

No form. The gallery crawls public repositories daily. Needed:

1. `gemini-extension.json` at the repository root (included).
2. The `gemini-cli-extension` topic on the GitHub repository (repo **About** → Topics).
3. A GitHub release tagged to match the manifest version (`v1.0.0`).

Install for users:

```bash
gemini extensions install https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research
```

Docs: https://geminicli.com/docs/extensions/releasing/

## Google Antigravity MCP Store

No form; developers post a listing request in the Google Antigravity category of the Google AI Developers Forum (tag `mcp`). Test the connection in Antigravity first: an older thread reported that OAuth-protected remote servers did not connect.

Suggested post:

> **[MCP Store] Listing request: DalalOS (remote MCP server, OAuth)**
>
> - **Server URL:** https://mcp.dalalos.in/mcp
> - **Transport:** streamable HTTP
> - **Auth:** OAuth 2.0 with PKCE and dynamic client registration (browser sign-in; free tier, no card)
> - **Official MCP Registry name:** `in.dalalos/mcp`
> - **Website / docs:** https://dalalos.in , https://dalalos.in/connect
> - **Source repository (public façade):** https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research
> - **Description:** Sourced Indian NSE/BSE stock research data: financials and mechanically-computed ratios, shareholding, filings, screens, FII/DII flows and macro context, each with source and freshness. Market-data tools are read-only; the only account writes are add/remove on the user's own watchlist. No investment advice, target prices or trading.
> - **Contact:** hello@dalalos.in

Antigravity declares a remote server with `serverUrl`:

```json
{
  "mcpServers": {
    "dalalos": {
      "serverUrl": "https://mcp.dalalos.in/mcp"
    }
  }
}
```

## Cursor

Cursor's marketplace takes a public GitHub repository holding a plugin (single-plugin layout: `.cursor-plugin/plugin.json` at the repository root, a committed logo, and `mcp.json` for the MCP server). Apply at https://cursor.com/marketplace/publish while signed in; the form asks for organization name, handle, a kebab-case namespace, contact email, a 1:1 SVG or PNG logo URL, description, the GitHub repository and website. Template and checklist: https://github.com/cursor/plugin-template . Reviews are manual and slow; submit once and do not resubmit while waiting. Questions: `marketplace-publishing@cursor.com`.

Suggested answers: organization `DalalOS`, namespace `@dalalos`, contact `hello@dalalos.in`, logo `https://dalalos.in/icon.svg`, website `https://dalalos.in`, repository `https://github.com/BeyondNoww/DalalOS-Indian-Stock-Research`, description from the Claude one-liner above.

## Community directories

Lower effort, optional: PulseMCP, mcp.so, Smithery (`smithery.ai/new` with the endpoint URL). Several import from the official MCP Registry entry, so a listing may already appear.

## Keep consistent

- Do not claim real-time prices, trading, or investment recommendations.
- Do not hard-code the tool count; use `capabilities.json`.
- After changing the endpoint, authentication or tool contract, update every listing above.
