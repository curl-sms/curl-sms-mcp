# curl-sms MCP server – send SMS from AI assistants

[![CI](https://github.com/curl-sms/curl-sms-mcp/actions/workflows/ci.yml/badge.svg)](https://github.com/curl-sms/curl-sms-mcp/actions/workflows/ci.yml)
[![MCP](https://img.shields.io/badge/MCP-Streamable_HTTP-0a0a0a)](https://modelcontextprotocol.io)
[![MIT license](https://img.shields.io/badge/license-MIT-0a0a0a)](LICENSE)

Let Claude, ChatGPT, Codex, Cursor, VS Code and other AI assistants send SMS through your [curl-sms](https://curl-sms.com) account. Say *"Text Anna that I'll be ten minutes late"* and the assistant does the rest.

The server is hosted: nothing to install, nothing to run.

| | |
|---|---|
| **URL** | `https://curl-sms.com/mcp` |
| **Transport** | Streamable HTTP (stateless, JSON responses) |
| **Sign-in** | OAuth 2.1 with your curl-sms account, or an API key as `Authorization: Bearer …` |
| **Cost and limits** | Same as the [API](https://curl-sms.com/pricing). With a test connection or test key nothing is sent or charged. |

This repository contains ready-to-copy configurations for every common client. The server itself is operated by curl-sms.

## Tools

| Tool | What it does |
|---|---|
| `send_sms` | Send an SMS to one number, optionally later (`send_at`) |
| `estimate_sms` | Encoding, parts and price of a text, without sending |
| `get_sms` | Status of one SMS |
| `list_sms` | The most recent SMS, optionally filtered by status or number |
| `cancel_sms` | Cancel a scheduled or queued SMS (refunded) |
| `get_account` | Plan, balance, included SMS and limits |

Numbers must be in international format (`+49151…`).

## Setup

### Claude (claude.ai, desktop and mobile)

1. **Settings → Connectors → Add custom connector**
2. Name `curl-sms`, URL `https://curl-sms.com/mcp`
3. **Connect**, sign in at curl-sms and choose **Live** or **Test**.

### Claude Code

```sh
claude mcp add --transport http curl-sms https://curl-sms.com/mcp
```

Then run `/mcp` and choose **Authenticate**. Or with an API key:

```sh
claude mcp add --transport http curl-sms https://curl-sms.com/mcp --header "Authorization: Bearer cs_test_…"
```

### ChatGPT

In developer mode: **Settings → Apps & Connectors → Create**, URL `https://curl-sms.com/mcp`, authentication **OAuth**, then approve at curl-sms.

### Codex

[`configs/codex/config.toml`](configs/codex/config.toml) → `~/.codex/config.toml`:

```toml
[mcp_servers.curl-sms]
url = "https://curl-sms.com/mcp"
bearer_token_env_var = "CURLSMS_API_KEY"
```

Or sign in with your account: `codex mcp login curl-sms`.

### Cursor

[`configs/cursor/mcp.json`](configs/cursor/mcp.json) → `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global):

```json
{
  "mcpServers": {
    "curl-sms": {
      "url": "https://curl-sms.com/mcp"
    }
  }
}
```

Cursor opens the curl-sms sign-in on first use. To use an API key instead, see [`configs/cursor/mcp.api-key.json`](configs/cursor/mcp.api-key.json).

### VS Code (GitHub Copilot)

[`configs/vscode/mcp.json`](configs/vscode/mcp.json) → `.vscode/mcp.json`:

```json
{
  "servers": {
    "curl-sms": {
      "type": "http",
      "url": "https://curl-sms.com/mcp"
    }
  }
}
```

### Windsurf

[`configs/windsurf/mcp_config.json`](configs/windsurf/mcp_config.json) → `~/.codeium/windsurf/mcp_config.json`.

### Gemini CLI

[`configs/gemini-cli/settings.json`](configs/gemini-cli/settings.json) → `~/.gemini/settings.json`.

### Any other client

Point it at `https://curl-sms.com/mcp` (Streamable HTTP). Clients with MCP OAuth support discover everything automatically:

- `https://curl-sms.com/.well-known/oauth-protected-resource/mcp`
- `https://curl-sms.com/.well-known/oauth-authorization-server`

OAuth 2.1 with PKCE (`S256`), dynamic client registration and rotating refresh tokens. Clients without OAuth send `Authorization: Bearer <api key>`.

## Safety

- An assistant sends a real SMS as soon as it calls `send_sms`. Most assistants ask first. Check receiver and text before you approve.
- Try it with a **test** connection or a test key (`cs_test_…`): every check runs, nothing is sent, nothing is charged.
- Daily and spend limits apply as with the API. Set them under [Billing](https://curl-sms.com/app/billing).
- See and disconnect connected assistants under [API keys → Connected apps](https://curl-sms.com/app/keys).

## Links

[curl-sms.com](https://curl-sms.com) · [MCP docs](https://curl-sms.com/docs/mcp) · [All integrations](https://curl-sms.com/integrations) · [API reference](https://curl-sms.com/docs/api) · [Pricing](https://curl-sms.com/pricing) · [Examples](https://github.com/curl-sms/curl-sms)

## License

The configuration examples in this repository are [MIT licensed](LICENSE). Use of the curl-sms service is subject to the [terms](https://curl-sms.com/terms).
