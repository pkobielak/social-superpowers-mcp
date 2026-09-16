# Changelog

## 0.2.0 — 2026-09-16

- **Authentication is required for every call.** An unauthenticated
  `initialize` now returns 401. In a client that supports MCP OAuth, add the URL
  and sign in with Google or GitHub when prompted — that is the whole setup.
  Clients without MCP OAuth send an API key, minted at
  <https://superpowers.social/account>, as `Authorization: Bearer sk_live_…` or
  `x-api-key`.
- Free tier available.
- Tool count corrected to 10 (5 X/Twitter, 5 Reddit). `reddit-get-subreddit-info`
  and `reddit-get-user-info` were listed in 0.1.0 but never shipped.
- README setup, arguments and examples corrected against the live tool schemas:
  `twitter-search` takes `query`, `count`, `cursor` and `text_only`.

## 0.1.0 — 2026-04-29

Initial public beta.

### Tools
- `twitter-search`, `twitter-read`, `twitter-thread`, `twitter-user-tweets`, `twitter-news`
- `reddit-search`, `reddit-get-post`, `reddit-get-posts`, `reddit-get-subreddit-info`, `reddit-get-user-info`, `reddit-get-user-posts`, `reddit-get-user-comments`

### Notes
- Streamable HTTP transport at `https://superpowers.social/mcp`.
- No authentication required during beta.
- Token-optimized response shaping (up to 90% smaller than raw API JSON).
