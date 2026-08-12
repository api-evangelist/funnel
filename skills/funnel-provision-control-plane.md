---
name: Provision Funnel workspaces, data sources and exports
description: >-
  Authenticate to the Funnel Control Plane API as a system user and create or update the objects that
  make up a Funnel setup — workspaces, data sources, custom dimensions/metrics and warehouse exports.
api: Funnel Control Plane API
surface: rest
servers:
  - https://controlplane.setup.us.funnel.io/v1
  - https://controlplane.setup.eu.funnel.io/v1
operations:
  - POST /v1/subscriptions/{subscription_id}/workspaces
  - GET /v1/subscriptions/{subscription_id}/workspaces/{id}
  - PUT /v1/subscriptions/{subscription_id}/workspaces/{id}
  - DELETE /v1/subscriptions/{subscription_id}/workspaces/{id}
  - POST /v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/datasources
  - PATCH /v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/datasources/{id}
  - POST /v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/custom-fields
  - POST /v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/exports
  - GET /v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/fields/{name}
generated: '2026-08-12'
method: generated
source: >-
  https://registry.terraform.io/providers/funnel-io/funnel/latest/docs +
  provider/funnel/funnel_client.go, provider/funnel/error_handler.go and provider/resources/*.go in
  github.com/funnel-io/terraform-provider-funnel + live probes of
  controlplane.setup.us.funnel.io on 2026-08-12
---

# Provision Funnel workspaces, data sources and exports

The Funnel Control Plane API is the write side of Funnel: it manages configuration, not data.
Funnel publishes **no OpenAPI** for it. Everything below is read from Funnel's own open-source
Terraform provider — which is the only client Funnel supports, and, as it turns out, the only client
the API will serve.

## Prefer Terraform

Unless you have a reason not to, drive this surface through the official provider:

```hcl
terraform {
  required_providers {
    funnel = { source = "funnel-io/funnel" }
  }
}

provider "funnel" {
  environment     = "us"          # us | eu | stage | dev
  subscription_id = var.subscription_id   # fsXXXXXXXXXXX
  client_id       = var.client_id
  client_secret   = var.client_secret
}
```

Resources: `funnel_workspace`, `funnel_data_source`, `funnel_custom_dimension`,
`funnel_custom_metric`, `funnel_bigquery_export`, `funnel_snowflake_export`, `funnel_gcs_export`,
`funnel_measurement_export`. Data sources: `funnel_workspace`, `funnel_export_field`.

## Calling the API directly

1. **Create a system user.** In the Funnel app: **Subscription overview > Authentication**. You get
   a `client_id` and `client_secret`.
2. **Get a token.** `POST https://login.funnel.io/oauth/token` with
   `grant_type=client_credentials`, your client credentials, and the audience matching your region —
   `https://controlplane.setup.us.funnel.io` or `https://controlplane.setup.eu.funnel.io`. A token
   minted for one region will not work against the other.
3. **Send the right headers on every request.**

   | Header | Value |
   | --- | --- |
   | `Authorization` | the bearer token |
   | `Content-Type` | `application/json` |
   | `Accept` | `application/json` |
   | `User-Agent` | `terraform-provider-funnel/<version>` |

   The `User-Agent` is **mandatory and enforced**, and it is documented nowhere. Anything else gets
   `400 {"error":"Invalid User-Agent header. Expected terraform-provider-funnel"}`, and a version
   below the server's floor gets
   `400 {"error":"terraform-provider-funnel version is too old. Minimum required: 0.1.0"}`. Treat
   this API as a private transport for Funnel's own tooling.

## Path shapes

Two nesting levels. Subscription-scoped:

```
/v1/subscriptions/{subscription_id}/workspaces[/{id}]
```

Workspace-scoped:

```
/v1/subscriptions/{subscription_id}/workspaces/{workspace_id}/{entity}[/{id}]
```

where `{entity}` is one of `datasources`, `custom-fields`, `exports`, `fields`.

Two of these are polymorphic and it matters:

- **`exports`** is one endpoint for all four destinations (BigQuery, Snowflake, GCS, measurement).
  The destination type lives in the body, not the path.
- **`custom-fields`** is one endpoint for both custom dimensions and custom metrics. Dimension
  `data_type` is `string | date | datetime`; metric `data_type` is
  `number | percent | monetary | duration`.

## Order of operations

1. `POST .../workspaces` — create the workspace.
2. `POST .../workspaces/{workspace_id}/datasources` — connect each platform. Supply either
   `remote_id` (the account ID in the source system) or, for connectors needing several identifiers,
   `remote_struct` as a JSON-encoded object such as
   `{"customer_id":"123","login_customer_id":"456"}` for Google Ads. **They are mutually exclusive,
   and changing either forces resource replacement.**
3. `POST .../workspaces/{workspace_id}/custom-fields` — add custom dimensions and metrics.
4. `GET .../workspaces/{workspace_id}/fields/{name}` — look up a field by NAME (not by id) to
   confirm what is exportable.
5. `POST .../workspaces/{workspace_id}/exports` — create the warehouse or measurement export.

## Update and delete semantics

- Update is **`PUT`** for workspaces, exports and custom-fields, but **`PATCH`** for datasources.
  There is no rule you can infer; check the entity.
- `DELETE` is effectively idempotent — the official client treats `404` on delete as success.
  Nothing else is idempotent: **there is no idempotency key**, so do not blindly retry a `POST`.

## Error handling

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 | Bad request; message in the `error` member | Fix the User-Agent or the body. Do not retry unchanged. |
| 401 | Unauthorized | Re-mint the token with the correct regional audience. |
| 403 | `Forbidden - limit reached` | **This is a quota signal, not a permissions signal.** You have run out of flexpoint capacity or plan entitlement. Adding a connector costs 50 FP, an account 5 FP, a visualization destination 150 FP, a warehouse destination 300 FP. |
| 404 | Not Found | Verify the id. On DELETE, treat as success. |
| 429 | Too Many Requests | Back off. No `Retry-After` and no rate-limit headers are published — use your own backoff. |
| 5xx | Server error | Retry with backoff. |

Errors are a flat `{"error": ...}` object where the value may be a string **or** a nested object.
There is no RFC 9457 `problem+json` and no stable error codes — you can only branch on HTTP status
and English message text.

## Regions

`us` -> `https://controlplane.setup.us.funnel.io/v1`, `eu` -> `https://controlplane.setup.eu.funnel.io/v1`.
The region selects the host **and** the token audience together. `stage` and `dev` exist in the
provider schema but are Funnel-internal, not a customer sandbox — there is no public sandbox.
