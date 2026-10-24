<p align="center"><img src="logo.png" width="96" height="96" alt="b59.link"></p>

# b59.link MCP server

Short links, QR codes and page-view trackers for AI agents, with click stats and email-code sign-up.

The b59.link MCP server gives an agent a link shortener and QR maker it can use without a person setting up an
account first. The agent can start straight away without signing in (at a low rate limit), or sign itself up with a
6-digit email code to get a session key and a higher limit.

This is a **hosted remote server** (Streamable HTTP). There is nothing to install or run: point your MCP client at the
URL below. This repository holds the docs, the logo and the registry entry; the server itself runs at b59.link.

- Endpoint: `https://b59.link/api/mcp`
- Docs: https://b59.link/mcp
- Website: https://b59.link

## Connect

Claude Code:
```bash
claude mcp add --transport http b59link https://b59.link/api/mcp
```

Claude Desktop, Cursor, Cline, VS Code and other clients that read an `mcpServers` block:
```json
{
  "mcpServers": {
    "b59link": {
      "type": "http",
      "url": "https://b59.link/api/mcp"
    }
  }
}
```

With a session key (after the `auth` tool), add a header:
```json
"headers": { "Authorization": "Bearer ${B59_SESSION_KEY}" }
```

## Tools

| Tool | What it does |
|---|---|
| `shorten_url` | Shorten a long URL (custom slug optional). Returns the short URL, a QR code and the stats page. |
| `generate_qr` | A trackable QR code for a URL, with its own short link so scans are counted. |
| `make_qr_image` | A plain QR image for any text (URL, Wi-Fi, phone, email). No short link, nothing to expire. |
| `create_tracker` | A cookie-free page-view tracker for a website, with ready-to-paste snippets. |
| `lookup_url` | Does this URL already have a short link? Handles http/https, www and trailing-slash variants. |
| `get_stats_url` | The stats page for a slug, short URL, destination or tracker. |
| `auth` | Sign in by email code (or with your account's API key) to raise the rate limit and own what you create. |

Every tool carries a `title` and `readOnlyHint` / `destructiveHint` / `openWorldHint` annotations. Stats are shown on
the website: the tools return stats page URLs, not raw visitor data.

## Sign-in without a person

1. Call `auth` with `email`: a 6-digit code goes to that address.
2. Call `auth` again with `email` and `otp`: you get a session key.
3. Send it as `Authorization: Bearer <key>` on later requests.

No key? Every tool still works at the anonymous limit.

## Check it from a terminal

```bash
curl -s https://b59.link/api/mcp -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'
```

## Links

- MCP Registry name: `link.b59/b59-link` (see `server.json`)
- Privacy: https://b59.link/privacy · Contact: support@biz59.net

Made by Biz59 LLC, New York. The docs in this repository are MIT licensed.
