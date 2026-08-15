# Contributing

This repository is the public integration and discovery surface for DalalOS. Contributions are welcome when they improve public documentation or metadata **without exposing the private service implementation**.

## Good contributions

Examples of appropriate changes:

- correct a broken or outdated public documentation link;
- improve MCP client connection instructions using publicly documented client behavior;
- clarify public capability boundaries, provenance or freshness semantics;
- improve safe example prompts;
- update public registry/directory metadata after an externally visible product change;
- fix spelling, accessibility or documentation formatting issues.

## Do not add private implementation material

Do **not** commit or describe:

- backend application source code or private repository contents;
- source-adapter, ingestion, scraper, parser or refresh implementation details;
- database, table, cache or storage schemas;
- deployment topology, host details, infrastructure configuration or operational runbooks;
- credentials, tokens, API keys, secrets or real authenticated request headers;
- private issues, pull requests, logs, incidents or internal debugging output;
- private analytics, usage metrics or unpublished model/evaluation results;
- unpublished architecture, security controls or implementation-specific failure modes;
- customer/account data or any other non-public information.

If a public document needs to explain how DalalOS behaves, describe the **externally observable contract** rather than how the backend implements it.

## Claims and capability changes

Public claims must stay within the product boundary:

- quote-style market data is end-of-day, not a live tick feed;
- DalalOS is for research data, not brokerage/order execution;
- DalalOS does not issue buy/sell recommendations or target prices;
- market-data research tools are read-only; the caller's own DalalOS watchlist is the explicit add/remove write exception;
- unavailable or stale coverage should remain explicit rather than being represented as a value that was not supplied.

Avoid hard-coded capability counts unless they are generated from a public source of truth. The private service evolves faster than manually copied marketing numbers.

## Registry metadata

`server.json` describes the hosted MCP service. Its `repository` field, if ever used, must refer to actual server source code as defined by the MCP Registry schema. This repository is intentionally not the backend source repository, so the public manifest currently uses `websiteUrl` instead.

The existing Registry identity is `io.github.aatharva16/dalalos`. Do not create a second Registry identity for the same hosted endpoint without an explicit migration plan.

## Pull request checklist

Before opening a PR, verify:

- [ ] The change contains no credential, private URL, private repository content or internal implementation detail.
- [ ] Every product claim is already public/externally observable or is backed by a public DalalOS reference.
- [ ] Client setup instructions defer to the maintained vendor/DalalOS docs when UI steps are likely to change.
- [ ] Examples contain placeholders only, never real keys/tokens/account identifiers.
- [ ] The change does not imply live market data, trade execution, investment advice or target-price capability.
- [ ] Registry metadata does not misrepresent this façade repository as backend source code.

For security-sensitive findings, follow [`SECURITY.md`](SECURITY.md) instead of opening a public issue.
