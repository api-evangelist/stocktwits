---
name: Post and engage on StockTwits
description: Create a message (twit) tagging a symbol and like community messages on behalf of a user.
api: openapi/stocktwits-openapi-original.json
operations: [createMessage, likeMessage, verifyAccount]
---

# Post and engage on StockTwits

Publish and interact on behalf of an authenticated StockTwits user.

## Auth
- Requires an OAuth 2.0 user access_token.
- `createMessage` needs the `publish_messages` scope; liking also uses `publish_messages`.
- Confirm the token with `verifyAccount` (`GET /account/verify.json`) before writing.

## Steps
1. Call `verifyAccount` to confirm the token and get the current user.
2. Create a twit with `createMessage` (`POST /messages/create.json`). Include the message `body`; cashtags like `$AAPL` in the body link to symbols.
3. Like a message with `likeMessage` (`POST /messages/like.json`) passing the target message `id`. Reverse with `unlikeMessage`.

## Conventions
- Writes are NOT declared idempotent — do not blindly retry a `createMessage` on a network timeout; verify first.
- Errors use `{ response: { status }, errors: [ { message } ] }`; handle 401 (bad/expired token), 422 (validation), 429 (rate limit).
