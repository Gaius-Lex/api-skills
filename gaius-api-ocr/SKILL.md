---
name: gaius-api-ocr
description: Documents OCR REST API (multipart file upload and poll for text). Use when integrating `/api/v1/ocr/poll`, engine selection, or rate limits on `OcrPollSubmitView`.
---

# OCR API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/api/views/ocr.py`

## `POST /api/v1/ocr/poll`

- `multipart/form-data`: `file`, opcjonalnie `engine` — `default` lub `google_doc_ai`
- Typy plików: lista w `OcrPollSubmitView.ALLOWED`

**202:** `{ "request_id": "<uuid>" }` · Throttling: `APIKeyDailyRateThrottle`

## `GET /api/v1/ocr/poll/<request_id>`

Sukces: m.in. `text`, `file_name`, `file_type`, `engine` (`format_success_result`).

## Autoryzacja

`Authorization: Api-Key <klucz>`
