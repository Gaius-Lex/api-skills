---
name: gaius-api-theses
description: Documents theses-analysis (BETA) webhook and polling. Use when integrating `/api/v1/theses-analysis`, legal thesis extraction, or comparing with `case-law-analysis` routes.
---

# Theses-analysis API (BETA)

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/views/views.py` — `ThesesAnalysisAPIWebhookView`, `ThesesAnalysisAPISubmitPollingJobView`, `ThesesAnalysisPollingGetView`

## Webhook

`POST /api/v1/theses-analysis` — body: `endpoint`, `temp_token`, `text` (msgspec)

## Polling

`POST /api/v1/theses-analysis/poll` — `text` → `request_id`

`GET /api/v1/theses-analysis/poll/<request_id>` — `text`, `theses_analysis` przy sukcesie

## Powiązanie

`api/v1/case-law-analysis` → `ThesesAnalysisAPIView` (inny przypadek niż polling powyżej).

## Autoryzacja

`Authorization: Api-Key <klucz>`
