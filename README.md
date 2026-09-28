# SecureSMTP MCP server

Connect [SecureSMTP](https://securessmtp.com) to Claude and other MCP clients. Send transactional
mail, manage sending domains, and read delivery analytics for your account — in natural language.

- **Endpoint:** `https://securessmtp.com/api/mcp` (Streamable HTTP)
- **Auth:** OAuth 2.1 (one-click, recommended) **or** a site API key as a Bearer token
- **Cost:** free — every call runs against your own account and stays within your plan limits

## What it can do

| Tool | What it does |
|------|--------------|
| `get_account` | Plan tier, email quota (used / limit / remaining), number of sending domains |
| `list_sending_domains` | List the account's sending domains and their verification status |
| `add_sending_domain` | Add a sending domain; returns the DNS records to publish |
| `verify_domain` | Verify a sending domain's DNS (SPF / DKIM) |
| `get_deliverability` | Delivery status counts for the last 30 days (delivered / opened / bounced / …) |
| `send_email` | Send a transactional email — enforces the same quota, spam scoring, and suppression as the API |

Every tool is scoped to a single site, so a connection can only ever see or act on that account.

## Connect from Claude Code

OAuth (opens a browser to approve):

```bash
claude mcp add --transport http securessmtp https://securessmtp.com/api/mcp
```

Or with a site API key (no browser step):

```bash
claude mcp add --transport http securessmtp https://securessmtp.com/api/mcp \
  --header "Authorization: Bearer <your-site-api-key>"
```

## Connect from claude.ai (custom connector)

Requires a claude.ai Pro/Team/Enterprise plan (custom connectors are not available on the free tier).

1. Settings → Connectors → Add custom connector
2. URL: `https://securessmtp.com/api/mcp`
3. Approve access when the SecureSMTP consent screen appears, and pick which site to grant.

## Where to get an API key

Sign in at [securessmtp.com](https://securessmtp.com), open a site, and copy its API key — the same key
the WordPress plugin and REST API use.

## How auth works

- **OAuth 2.1** with PKCE and dynamic client registration. Discovery lives at
  `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server`.
  Access tokens are short-lived; refresh tokens rotate.
- **API key** — send the site key as `Authorization: Bearer <key>`.

## Links

- Website: https://securessmtp.com
- API docs: https://securessmtp.com/docs/api
- Support: https://securessmtp.com/contact
