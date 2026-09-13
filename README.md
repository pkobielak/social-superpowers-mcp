# Superpowers.social — MCP server for X/Twitter & Reddit

> **Status (2026-09-13): anonymous access is paused** after abuse of the free anonymous tier.
> Access now requires a free API key. **New users: email
> [hello@superpowers.social](mailto:hello@superpowers.social?subject=Free%20API%20key) and you
> get one, free.** Send it as `Authorization: Bearer <key>` (or `x-api-key`) with the URL below.

A hosted [MCP](https://modelcontextprotocol.io) server that gives Claude, ChatGPT, Cursor, and any agent runtime live tool-use access to **X/Twitter** and **Reddit**. No X/Reddit API keys. No scraping setup. No banned accounts.

**Endpoint:** `https://superpowers.social/mcp` (Streamable HTTP)

[![Status](https://img.shields.io/badge/status-anonymous%20access%20paused-orange)](https://superpowers.social)
[![Free](https://img.shields.io/badge/pricing-free%20API%20key%20by%20email-green)](mailto:hello@superpowers.social?subject=Free%20API%20key)
[![MCP](https://img.shields.io/badge/protocol-MCP-purple)](https://modelcontextprotocol.io)
[![smithery badge](https://smithery.ai/badge/pkobielak/social-superpowers)](https://smithery.ai/servers/pkobielak/social-superpowers)

---

## Why

The official APIs are paywalled into oblivion (X starts at $100/mo, Reddit is heavily rate limiting). Direct scraping flags accounts in days. Most agents end up stuck on stale article snippets while the actual conversation happens on X and Reddit.

This server runs the gateway so you don't have to. One URL, ten tools, schema-validated responses, token-optimized payloads.

## Tools

### X/Twitter (5)
| Tool | What it does |
|---|---|
| `twitter-search` | Search tweets by query with engagement metrics |
| `twitter-read` | Read a single tweet by URL or ID |
| `twitter-thread` | Read a full conversation thread |
| `twitter-user-tweets` | Recent tweets from a user |
| `twitter-news` | Trending news on X |

### Reddit (5)
| Tool | What it does |
|---|---|
| `reddit-search` | Search posts across Reddit or one subreddit |
| `reddit-get-post` | Get a post + comments |
| `reddit-get-posts` | Get hot/new/top posts from a subreddit |
| `reddit-get-user-posts` | Posts submitted by a user |
| `reddit-get-user-comments` | Comments by a user |

All tools are read-only, idempotent, and return token-optimized responses (up to 90% smaller than raw API JSON).

## Install

### Claude Desktop
Settings → Connectors → **Add custom connector**:
- **Name:** `social-superpowers`
- **URL:** `https://superpowers.social/mcp`

### ChatGPT (desktop or web)
Settings → Apps & Connectors → enable **Developer mode** (one-time) → **Create**:
- **Name:** `social-superpowers`
- **URL:** `https://superpowers.social/mcp`

All tools are read-only, so the connector works on Plus/Pro developer mode.

### Claude Code
```bash
claude mcp add --transport http social-superpowers https://superpowers.social/mcp
```

### Cursor
Settings → MCP → Add new MCP Server:
```json
{
  "social-superpowers": {
    "type": "http",
    "url": "https://superpowers.social/mcp"
  }
}
```

### Any MCP client (`mcp.json`)
```json
{
  "mcpServers": {
    "social-superpowers": {
      "type": "http",
      "url": "https://superpowers.social/mcp"
    }
  }
}
```

## Examples

```
You: search X for what people are saying about $INTC catalysts this week
Agent: [calls twitter-search with q="$INTC", time_range="7d"]
       → returns 25 tweets with sentiment, engagement, top replies

You: which subreddits discuss the Tirzepatide side-effects?
Agent: [calls reddit-search → reddit-get-posts → reddit-get-post]
       → grounded answer with permalinks
```

## Pricing

**Free, with an API key.** No OAuth. No credit card. Email
[hello@superpowers.social](mailto:hello@superpowers.social?subject=Free%20API%20key) with one line
about what you are building and you get a key with a daily call allowance. Higher allowances on request.

## Status & Limits

- **Anonymous access is paused** since 2026-09-13 after abuse of the free anonymous tier.
  Calls without a key are rejected.
- Uptime / status: [superpowers.social](https://superpowers.social)
- Keyed access has a per-key daily call limit. Contact the address above for more.

## Specs

- **Transport:** Streamable HTTP (`https://superpowers.social/mcp`)
- **Auth:** API key, `Authorization: Bearer <key>` or `x-api-key: <key>` header (free by email, see Pricing)
- **Spec version:** MCP 2025-12-11
- **Source platforms:** X/Twitter, Reddit

## License

MIT — this repository contains the public manifest, examples, and docs. The hosted gateway implementation is closed-source; the MCP contract is the integration surface.

## Links

- Site: <https://superpowers.social>
- MCP endpoint: <https://superpowers.social/mcp>
- Issues / feature requests: open an issue on this repo
- Agent-readable reference: <https://superpowers.social/llms.txt>
- MCP spec: <https://modelcontextprotocol.io>
