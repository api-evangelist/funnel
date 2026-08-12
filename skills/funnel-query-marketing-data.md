---
name: Query Funnel marketing data over MCP
description: >-
  Connect to Funnel's hosted MCP server and answer cross-channel marketing questions from a Funnel
  workspace — pick a workspace, load its context, discover fields, then query aggregated or raw data.
api: Funnel MCP
surface: mcp
servers:
  - https://mcp.ai.funnel.io/mcp
  - https://mcp.eu.ai.funnel.io/mcp
operations:
  - list_workspaces
  - load_workspace
  - search_fields
  - get_dimension_values
  - query_data
  - prepare_data
generated: '2026-08-12'
method: generated
source: >-
  https://help.funnel.io/en/articles/15014203-quick-start-guide-using-funnel-mcp +
  https://help.funnel.io/en/articles/15170890-connect-funnel-mcp-to-claude-as-a-custom-connector +
  https://mcp.ai.funnel.io/.well-known/oauth-protected-resource/mcp
---

# Query Funnel marketing data over MCP

Funnel MCP exposes one workspace's harmonized, cross-channel marketing data to any MCP client.
Every tool is **read-only** — nothing here can create or change a data source, field, dashboard or
export. If you need to change configuration, that is the Control Plane API, a different surface
entirely (see `funnel-provision-control-plane.md`).

## Connect

Pick the server for the region the customer's data lives in. **Do not guess the URL** — Funnel says
so explicitly, and the two regions are isolated for data residency.

| Data resides | Funnel app | MCP server URL |
| --- | --- | --- |
| Global (default) | `https://app.funnel.io/` | `https://mcp.ai.funnel.io/mcp` |
| EU | `https://app.eu.funnel.io/` | `https://mcp.eu.ai.funnel.io/mcp` |

The authoritative URL for a given account is shown in the Funnel app under **Funnel AI > MCP**.

Authentication is OAuth 2.0 authorization code against `https://login.funnel.io`. Most clients
register dynamically at `https://login.funnel.io/oidc/register` and need no configuration; clients
that cannot do dynamic registration (GitHub Copilot, for example) take a static OAuth client ID from
**Funnel AI > MCP > Advanced**. Scopes are `openid profile email offline_access` — request
`offline_access` or the connection will expire and require a browser sign-in again.

An unauthenticated call returns:

```json
{"jsonrpc":"2.0","id":0,"error":{"code":-32001,"message":"Missing Bearer token"}}
```

That is HTTP 401 and means reconnect, not retry.

## Steps

1. **Pick a workspace.** Call `list_workspaces` to see what this user can reach. A user may have
   access to many; never assume one.
2. **Load context.** Call `load_workspace` for the chosen workspace. This returns the connected
   platforms and the available fields, and is what makes later answers use the customer's own
   business definitions rather than generic ad-platform vocabulary. Do this before any query.
3. **Discover fields.** Call `search_fields` to find the dimensions, measures and metrics that
   actually exist in this workspace. Do not assume a field name from another Funnel account or from
   an ad platform's own API — Funnel's semantic layer is per-workspace.
4. **Resolve dimension values** when the question names a specific campaign, country, brand or
   channel: call `get_dimension_values` for that dimension and match against real values rather
   than guessing a string.
5. **Query.**
   - `query_data` for aggregated analysis, trends and cross-channel comparison. Documented
     parameters: `dimensions`, `measures`, `filter`, `group_by`, `order_by`, `limit`.
   - `prepare_data` for raw, row-level data with no aggregation, when you need to slice it yourself.

## Conventions and gotchas

- **Tool order is advisory.** Funnel notes the client, not the server, chooses the order. The
  sequence above is Funnel's documented happy path; follow it unless the user's request makes a step
  pointless.
- **`query_data` has a known open schema defect.** Funnel publicly acknowledges that the server has
  published an incomplete parameter schema for `query_data` (`"properties": {}`), which makes strict
  clients serialize array/object parameters as strings and the tool reject them with type validation
  errors. If `query_data` fails with type validation errors while the other tools work, that is this
  defect. Start a new session so the client reloads tool definitions, then reconnect and re-do OAuth;
  if it still fails, it is a server-side issue for Funnel support, not a prompt problem. Fall back to
  `prepare_data` and aggregate locally.
- **Read-only means read-only.** If a user asks you to add a connector, create a custom dimension or
  change an export, tell them it cannot be done over MCP and point at the Funnel app or the
  Terraform provider.
- **No rate limits are published** and MCP does not consume flexpoints, but there is no
  `Retry-After` or rate-limit header to plan against. Keep query volume sane.
- **Availability**: all Funnel plans.

## Worked prompts

- `Show me trends for spend, impressions, clicks, CPC and revenue, broken down by traffic source, for the last six months.`
- `Show month-to-date spend by channel against monthly budget. Flag any channel pacing more than 10% over.`
- `CPA on Google Ads spiked last week. Which campaigns and keywords are driving the change?`

Verify a fresh connection with `List the workspaces I can access in Funnel.`
