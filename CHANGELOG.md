# Changelog

## 2026-09-13

- Anonymous access paused after abuse of the free anonymous tier. Access now requires a free API key, issued by email (hello@superpowers.social).

## 0.1.0 — 2026-04-29

Initial public beta.

### Tools
- `twitter-search`, `twitter-read`, `twitter-thread`, `twitter-user-tweets`, `twitter-news`
- `reddit-search`, `reddit-get-post`, `reddit-get-posts`, `reddit-get-subreddit-info`, `reddit-get-user-info`, `reddit-get-user-posts`, `reddit-get-user-comments`

### Notes
- Streamable HTTP transport at `https://superpowers.social/mcp`.
- No authentication required during beta.
- Token-optimized response shaping (up to 90% smaller than raw API JSON).

## 0.2.0 — 2026-07-30

- Tool set is now 10 tools: removed `reddit-get-subreddit-info` and
  `reddit-get-user-info` (no reliable anonymous data source; they only
  returned errors). Agents should use `reddit-search` + `reddit-get-posts`.
- Added ChatGPT setup (custom connectors via developer mode; read-only
  tools work on Plus/Pro).
- Added agent-readable service reference at https://superpowers.social/llms.txt
- Reliability: hard 30s per-call deadline; cleaner error messages;
  faster `twitter-user-tweets`.
