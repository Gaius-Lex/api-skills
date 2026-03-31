---
name: gaius-api-doc-verification
description: Documents document verification polling API (citations/signatures checks). Use when integrating `/api/v1/doc-verification/poll`, `fileId` vs inline `content`, or source toggles from `doc_verification.py`.
---

# Doc Verification API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/api/views/doc_verification.py`

## `POST /api/v1/doc-verification/poll`

- `content` **albo** `fileId` / `file_id` (plik z katalogu external organizacji)
- `deep_check`, `check_judiciary`, `check_interpretations`, `check_acts` (alias `check_codexes`), `check_kio` — parsowanie bool w `_as_bool`

**202:** `{ "request_id": "<uuid>" }`

## `GET /api/v1/doc-verification/poll/<request_id>`

Jak `BasePollingGetView`.

## Autoryzacja

`Authorization: Api-Key <klucz>`
