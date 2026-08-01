---
name: Read symbol sentiment from StockTwits
description: Pull the real-time message stream and trending signal for a ticker to gauge community sentiment.
api: openapi/stocktwits-openapi-original.json
operations: [getSymbolStream, searchSymbols, getTrendingSymbols]
---

# Read symbol sentiment from StockTwits

Use the StockTwits v2 API to read what the community is saying about a ticker.

## Auth
- Public reads work with an app-level `access_token` query parameter.
- For a user's home/friends streams, use an OAuth 2.0 user token with the `read` scope.

## Steps
1. If you only have a company name, resolve it to a ticker with `searchSymbols` (`GET /search/symbols.json?q=...`).
2. Fetch the ticker's stream with `getSymbolStream` (`GET /streams/symbol/{symbol}.json`). Page with `since` / `max` (id cursor) and `limit` (max 30). Optionally set `filter=links|charts|videos`.
3. For market-wide context, call `getTrendingSymbols` (`GET /trending/symbols.json`).

## Conventions
- Pagination is id-based: pass the largest id you have as `max` to page backward, or `since` to poll forward.
- Errors use `{ response: { status }, errors: [ { message } ] }`; handle 404 (unknown symbol), 422 (missing symbol), 429 (rate limit — back off).
