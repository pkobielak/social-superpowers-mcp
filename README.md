# Superpowers.social — MCP server for X/Twitter & Reddit

A hosted [MCP](https://modelcontextprotocol.io) server that gives Claude, ChatGPT, Cursor, and any agent runtime live tool-use access to **X/Twitter** and **Reddit**. No API keys. No scraping setup. No banned accounts.

**Endpoint:** `https://superpowers.social/mcp` (Streamable HTTP)

[![Status](https://img.shields.io/badge/status-public%20beta-blue)](https://superpowers.social)
[![Free](https://img.shields.io/badge/pricing-free%20during%20beta-green)](https://superpowers.social)
[![MCP](https://img.shields.io/badge/protocol-MCP-purple)](https://modelcontextprotocol.io)
[![smithery badge](https://smithery.ai/badge/pkobielak/social-superpowers)](https://smithery.ai/servers/pkobielak/social-superpowers)

---

## Why

The official APIs are paywalled into oblivion (X starts at $100/mo, Reddit is heavily rate limiting). Direct scraping flags accounts in days. Most agents end up stuck on stale article snippets while the actual conversation happens on X and Reddit.

This server runs the gateway so you don't have to. One URL, twelve tools, schema-validated responses, token-optimized payloads.

## Tools

### X/Twitter (5)
| Tool | What it does |
|---|---|
| `twitter-search` | Search tweets by query with engagement metrics |
| `twitter-read` | Read a single tweet by URL or ID |
| `twitter-thread` | Read a full conversation thread |
| `twitter-user-tweets` | Recent tweets from a user |
| `twitter-news` | Trending news on X |

### Reddit (7)
| Tool | What it does |
|---|---|
| `reddit-search` | Search posts across Reddit |
| `reddit-get-post` | Get a post + comments |
| `reddit-get-posts` | Get posts from a subreddit |
| `reddit-get-subreddit-info` | Subreddit metadata |
| `reddit-get-user-info` | Reddit user profile |
| `reddit-get-user-posts` | Posts submitted by a user |
| `reddit-get-user-comments` | Comments by a user |

All tools are read-only, idempotent, and return token-optimized responses (up to 90% smaller than raw API JSON).

## Install

### Claude Desktop
Settings → Connectors → **Add custom connector**:
- **Name:** `social-superpowers`
- **URL:** `https://superpowers.social/mcp`

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
Agent: [calls reddit-search → reddit-get-subreddit-info → reddit-get-posts]
       → grounded answer with permalinks
```

## Pricing

**Free during public beta.** No OAuth. No login. No credit card required.

## Status & Limits

- Uptime / status: [superpowers.social](https://superpowers.social)
- Soft per-IP rate limits during beta to keep the service open. Contact for higher limits.

## Specs

- **Transport:** Streamable HTTP (`https://superpowers.social/mcp`)
- **Auth:** none required during beta
- **Spec version:** MCP 2025-12-11
- **Source platforms:** X/Twitter, Reddit

## License

MIT — this repository contains the public manifest, examples, and docs. The hosted gateway implementation is closed-source; the MCP contract is the integration surface.

## Links

- Site: <https://superpowers.social>
- MCP endpoint: <https://superpowers.social/mcp>
- Issues / feature requests: open an issue on this repo
- MCP spec: <https://modelcontextprotocol.io>
