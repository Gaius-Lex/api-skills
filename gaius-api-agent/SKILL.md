---
name: gaius-api-agent
description: Documents Gaius Agent REST endpoints (workflow, `/api/v1/agent`, agent-profiles, webhook vs polling). Use when integrating the tool-using legal agent, debugging workflow callbacks, or mapping EchoAPI fields to `workflow.py`.
---

# Gaius Agent API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Base URL:** `https://api.gaius-lex.pl`

**Related skills:** see [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md).

EchoAPI export may use placeholder keys (`key_0`). Real field names match `WorkflowWebhookRequest` / `WorkflowPollingRequest` in `legalgpt/gaius/api/views/workflow.py`.

## Authorization

`Authorization: Api-Key <organization_key>`

For `POST`: `Content-Type: application/json`; `Accept: application/json`.

## Webhook vs polling

| Mode | Start | Result |
|------|--------|--------|
| Webhook | `POST /api/v1/agent` | Server calls your `endpoint` (optional `temp_token`) when done. |
| Polling | `POST /api/v1/agent/poll` → `request_id` | `GET /api/v1/agent/poll/<uuid>` until not processing. |

## `GET /api/v1/agent-profiles`

Agent profile list (legacy alias: `GET /api/v1/user-hats`).

Response includes `status`, `agent_profiles`, and `user_hats` (compatibility), without `injected_prompt` in the dump.

## `POST /api/v1/agent` (webhook)

`BaseWebhookRequest` fields plus: `messages`, optional `documents`, `max_tool_calls` (default 5), `max_lex_budget_agent` (default 50), `permission_mode` (`always_allow` | `ask_permission`), `profile_id`, `source_filters`, `exclude_file_inputs_from_tools`.

Typical response: `202` with `status` (e.g. processing), `401` on bad key.

## `POST /api/v1/agent/poll`

Same fields as webhook **except** `endpoint` and `temp_token`. Polling defaults: `max_tool_calls=20`, `max_lex_budget_agent=500`.

`202` body: `{ "request_id": "<uuid>" }`.

## `GET /api/v1/agent/poll/<request_id>`

- `202` — still processing
- `200` — success (`status`, `result` per `WorkflowAPIStatusPoll`)
- `404` / `500` — not found / error

`request_id` is a real UUID (not the OpenAPI template `{{agent_request_id}}`).

## Routes

Registered in `gaius/urls.py` under `/api/v1/agent*` and `/api/v1/agent-profiles` (and legacy `/api/v1/user-hats`). Docstrings may mention `/api/v1/workflow*`; that path is not registered there—use `/api/v1/agent`.

## Code

- `legalgpt/gaius/api/views/workflow.py`
- `legalgpt/gaius/urls.py`
- `workflows/types.py` — `PermissionMode`
