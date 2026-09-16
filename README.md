# Superpowers.social — MCP server for X/Twitter & Reddit

Hosted [MCP](https://modelcontextprotocol.io) server for live X/Twitter and Reddit
research. 10 read-only tools for search, threads, timelines, posts and comments.
Streamable HTTP: add the URL and sign in with Google or GitHub when prompted.
Free tier available.

- **Website:** <https://superpowers.social>
- **Endpoint:** `https://superpowers.social/mcp` (Streamable HTTP)

[![Status](https://img.shields.io/badge/status-live-brightgreen)](https://superpowers.social)
[![Pricing](https://img.shields.io/badge/pricing-free%20tier-green)](https://superpowers.social)
[![MCP](https://img.shields.io/badge/protocol-MCP-purple)](https://modelcontextprotocol.io)

---

## Tools

All ten tools are read-only and idempotent. Eight of them trim the upstream
response to the fields listed below and, when the trim saves enough to be worth
reporting, append a stats line saying how much was stripped. `twitter-news` and
`reddit-get-user-comments` are the exceptions: they return the upstream objects
as they arrive, so the field lists below do not describe them and no stats line
is emitted.

### X/Twitter (5)

| Tool | What it does | Arguments |
|---|---|---|
| `twitter-search` | Search tweets matching a query | `query` (required), `count` (default 10), `cursor`, `text_only` |
| `twitter-read` | Read a single tweet | `tweet` (URL or ID, required), `text_only` |
| `twitter-thread` | Read a full conversation thread | `tweet` (URL or ID, required), `cursor`, `text_only` |
| `twitter-user-tweets` | Recent tweets from one account | `handle` (required, no `@`), `count` (default 20), `cursor`, `text_only` |
| `twitter-news` | Trending news on X | `count` (default 10), `category` (`for-you`, `news`, `sports`, `entertainment`, `trending`) |

Tweet fields, for `twitter-search`, `twitter-read`, `twitter-thread` and
`twitter-user-tweets`: `id`, `text`, `createdAt`, `author`, `authorName`, `url`,
and `replyCount` / `retweetCount` / `likeCount` when X returns them. Quoted
tweets are nested under `quotedTweet`. `text_only` reduces each tweet to
`{id, text}`. `twitter-news` returns news items, not tweets, and passes their
fields through unchanged apart from a curation prefix stripped off `category`.

### Reddit (5)

| Tool | What it does | Arguments |
|---|---|---|
| `reddit-search` | Search posts across Reddit or one subreddit | `query` (required), `subreddit`, `count` (default 10), `sort`, `time_filter`, `text_only` |
| `reddit-get-post` | A post plus its comments | `post_id` (required), `subreddit`, `comment_limit` (default 10) |
| `reddit-get-posts` | Top posts from a subreddit for a time window | `subreddit` (required), `count` (default 10), `time_filter` (default `week`), `text_only` |
| `reddit-get-user-posts` | Posts submitted by a user | `username` (required), `count` (default 10), `sort` (default `new`), `time_filter` (default `all`), `text_only` |
| `reddit-get-user-comments` | Comments by a user | `username` (required), `count` (default 10), `sort` (default `new`), `time_filter` (default `all`) |

Post fields: `id`, `title`, `author`, `subreddit`, `createdUtc`, `url`, and the
post body when there is one. The list tools — `reddit-search`,
`reddit-get-posts`, `reddit-get-user-posts` — truncate that body to a
500-character `selftext_preview`; `reddit-get-post` returns the full `selftext`
and its comments as `id`, `author`, `body`, plus `depth` where the source gives
one. `text_only` reduces each post to `{id, title}`. None of these carry a
Reddit score or comment count — the source does not provide them.
`reddit-get-user-comments` applies no field filtering — its comment objects come
through as the source returns them.

## Setup

Authentication is required for every call — an unauthenticated `initialize`
returns 401.

### Claude Code (verified 2026-09-16)

```bash
claude mcp add --transport http social-superpowers https://superpowers.social/mcp
```

Sign in with Google or GitHub when Claude Code opens the browser prompt. That is
the whole setup — the ten tools are then available in the session.

### Any other MCP client

Any client that speaks Streamable HTTP can connect to
`https://superpowers.social/mcp` with either:

- **its own MCP OAuth sign-in flow**, if it supports one — the same Google or
  GitHub prompt, nothing to set up in advance; or
- **an API key**, for clients without MCP OAuth: mint a key at
  <https://superpowers.social/account> and send it as
  `Authorization: Bearer sk_live_…` or `x-api-key`.

Generic Streamable-HTTP-with-API-key configuration shape (adjust to your
client's own config format):

```json
{
  "mcpServers": {
    "social-superpowers": {
      "type": "http",
      "url": "https://superpowers.social/mcp",
      "headers": {
        "Authorization": "Bearer sk_live_…"
      }
    }
  }
}
```

## Examples

```
You: what are people on X saying about $INTC?
Agent: twitter-search { "query": "$INTC", "count": 25 }
       → up to 25 tweets with author, timestamp, link and reply/retweet/like counts

You: which subreddits discuss Tirzepatide side effects?
Agent: reddit-search { "query": "tirzepatide side effects", "count": 10 }
       → posts with author, subreddit, permalink and a body preview
       reddit-get-post { "post_id": "1abc234" }
       → the full post with its comments
```

## License

MIT — this repository contains the public manifest, examples, and docs. The
hosted gateway implementation is closed-source; the MCP contract is the
integration surface.

## Links

- Site: <https://superpowers.social>
- MCP endpoint: <https://superpowers.social/mcp>
- MCP spec: <https://modelcontextprotocol.io>
