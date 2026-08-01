---
name: Manage a StockTwits watchlist
description: Create a watchlist, add symbols, and read its live message stream.
api: openapi/stocktwits-openapi-original.json
operations: [getWatchlists, createWatchlist, addSymbolToWatchlist, getWatchlistStream]
---

# Manage a StockTwits watchlist

Build and monitor a watchlist for an authenticated user.

## Auth
- OAuth 2.0 user token with the `publish_watch_lists` scope for writes; `read` for the stream.

## Steps
1. List existing watchlists with `getWatchlists` (`GET /watchlists.json`) to avoid duplicates.
2. Create one with `createWatchlist` (`POST /watchlists/create.json`, `name` in the body).
3. Add tickers with `addSymbolToWatchlist` (`POST /watchlists/{watchlist_id}/symbols/create.json`, `symbols` = comma-separated tickers).
4. Read the aggregated stream with `getWatchlistStream` (`GET /streams/watchlist/{watchlist_id}.json`), paging via `since`/`max`/`limit`.

## Conventions
- Symbol references use ticker strings (e.g. AAPL, TSLA).
- Errors use `{ response: { status }, errors: [ { message } ] }`; handle 401/404/422/429.
