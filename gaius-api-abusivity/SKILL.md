---
name: gaius-api-abusivity
description: Documents abusivity (BETA) webhook and polling APIs. Use when integrating `/api/v1/abusivity`, risk payloads, or debugging `AbusivityWebhookView` / polling views in `views.py`.
---

# Abusivity API (BETA)

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/views/views.py` — `AbusivityWebhookView`, `AbusivitySubmitPollingJobView`, `AbusivityPollingGetView`

## `POST /api/v1/abusivity` (webhook)

`endpoint`, `temp_token`, `text` → `202` processing

## `POST /api/v1/abusivity/poll`

`text` → `202` + `request_id`

## `GET /api/v1/abusivity/poll/<request_id>`

`200`: `text`, `abusivity_risk` · `202` pending · `500` error

## Autoryzacja

`Authorization: Api-Key <klucz>`
