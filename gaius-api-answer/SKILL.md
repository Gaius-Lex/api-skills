---
name: gaius-api-answer
description: Documents Answer API (research engine) webhook and polling. Use when integrating `/api/v1/answer`, callbacks, or `question`/`simplify` payloads from `answer.py`.
---

# Answer API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/api/views/answer.py`

## Przepływ

| Tryb | Endpoint |
|------|----------|
| Webhook | `POST /api/v1/answer` |
| Polling | `POST /api/v1/answer/poll` → `GET /api/v1/answer/poll/<uuid>` |

## Body

**Webhook** (`AnswerWebhookRequest`): `endpoint`, `temp_token`, `question`, `simplify` (domyślnie `false`).

**Polling:** `question`, `simplify` — bez `endpoint` / `temp_token`.

## Kanoniczne ścieżki

`gaius/urls.py`: `api/v1/answer`, `api/v1/answer/poll`, `api/v1/answer/poll/<uuid>` (ignoruj stare ścieżki w docstringach).
