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

## Przykładowe requesty

**Jak buduje się `messages`:** backend bierze **`messages[-1]["message"]`** jako treść bieżącego pytania, a **`messages[:-1]`** przekazuje jako historię do `_prepare_messages` (`main_agent.py`). Ostatni słownik musi więc mieć klucz **`message`** (inaczej `KeyError`). Historia: `role` `human` / `user`, `assistant` / `gaius`, opcjonalnie `tool` z `tool_call_id` i `content` — jak w `MainAgentConsumer.process_receive`.

**`profile_id`:** opcjonalny string (np. z `GET /api/v1/agent-profiles`). W `gaius/api/tasks/workflow.py` mapuje się na `UserHatConfig` tak jak w WebSocketcie (`DEFAULT_USER_HATS`); brak lub nieznany ID → domyślny profil (pierwszy z mapy).

**Webhook** — wymagany jest `endpoint` (URL callbacku po zakończeniu):

```bash
curl -sS -X POST 'https://api.gaius-lex.pl/api/v1/agent' \
  -H 'Authorization: Api-Key YOUR_ORG_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "endpoint": "https://twoj-serwer.example/agent-callback",
    "temp_token": "opcjonalny-sekret-do-weryfikacji-callbacku",
    "messages": [
      { "role": "human", "message": "Czy fotowoltaika to budowla?" }
    ]
  }'
```

**Polling** — start (w odpowiedzi `202` dostaniesz `request_id`):

```bash
curl -sS -X POST 'https://api.gaius-lex.pl/api/v1/agent/poll' \
  -H 'Authorization: Api-Key YOUR_ORG_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "messages": [
      { "role": "human", "message": "Czy fotowoltaika to budowla?" }
    ]
  }'
```

**Polling** — odczyt statusu (zamień `REQUEST_ID` na UUID z poprzedniej odpowiedzi):

```bash
curl -sS 'https://api.gaius-lex.pl/api/v1/agent/poll/REQUEST_ID' \
  -H 'Authorization: Api-Key YOUR_ORG_API_KEY'
```

Opcjonalnie: `documents` (kształt jak `GLexIDDocument` / `FileIDDocument` w `workflows/message_management/message_types.py`), `max_tool_calls`, `max_lex_budget_agent`, `permission_mode`, `profile_id`, `source_filters`, `exclude_file_inputs_from_tools` — pola modeli w `legalgpt/gaius/api/views/workflow.py`.

## Odpowiedzi HTTP (kod)

- **`POST /api/v1/agent`:** `202` i body `{"status": "processing"}` (`BaseWebhookAPIView`). Wynik nie jest w tej odpowiedzi — przychodzi **callbackiem** `POST` na Twój `endpoint` (JSON m.in. `messages`, `documents`, `result` — string odpowiedzi lub komunikat błędu, `temp_token`, `metadata`), zob. `try_workflow_webhook` w `gaius/api/tasks/workflow.py`.
- **`POST /api/v1/agent/poll`:** `202`, body `{"request_id": "<uuid>"}`.
- **`GET /api/v1/agent/poll/<request_id>`:** przy `status=pending` w DB → `202`, `{"status": "processing", "message": "Your request is still being processed. Please wait."}`; sukces → `200`, `{"status": "success", "result": { ... }}` (`result` ustawiane w `try_workflow_polling`, m.in. `answer`, `model_answers`, `metadata`); błąd → `500`, `{"status": "error", "reason": "..."}` (`BasePollingGetView`).

**Anonimizacja tekstu** to osobny endpoint: [gaius-api-anonymization](../gaius-api-anonymization/SKILL.md) (`POST /api/v1/anonymization`).

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

Typical response: `202` and `{"status": "processing"}`; `401` on bad key. Final payload is delivered to `endpoint` (see above).

## `POST /api/v1/agent/poll`

Same fields as webhook **except** `endpoint` and `temp_token`. Polling defaults: `max_tool_calls=20`, `max_lex_budget_agent=500`.

`202` body: `{ "request_id": "<uuid>" }`.

## `GET /api/v1/agent/poll/<request_id>`

- `202` — still processing (`status`: `processing`, plus `message`)
- `200` — `{"status": "success", "result": ...}` (`result` from `WorkflowPollGetView.format_success_result`)
- `404` — unknown `request_id` or wrong API key
- `500` — workflow error (`status`: `error`, `reason`)

`request_id` is a real UUID (not the OpenAPI template `{{agent_request_id}}`).

## Routes

Registered in `gaius/urls.py` under `/api/v1/agent*` and `/api/v1/agent-profiles` (and legacy `/api/v1/user-hats`). Docstrings may mention `/api/v1/workflow*`; that path is not registered there—use `/api/v1/agent`.

## Code

- `legalgpt/gaius/api/views/workflow.py`
- `legalgpt/gaius/api/tasks/workflow.py`
- `legalgpt/gaius/api/base.py`
- `legalgpt/gaius/urls.py`
- `workflows/types.py` — `PermissionMode`
