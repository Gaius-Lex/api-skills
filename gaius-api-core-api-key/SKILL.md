---
name: gaius-api-core-api-key
description: Documents creating child API keys via core_api. Use when integrating `POST /core_api/api-key/create`, `HasCreateKeyOrganizationAPIKey`, or comparing with dev-only key endpoints.
---

# core_api — tworzenie klucza API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en)

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Ścieżka:** `POST /core_api/api-key/create`

**Kod:** `legalgpt/core/views.py` — `CreateApiKey` · Mount: `legalgpt/urls.py` → `core_api/`

## Body

`{ "name": "<string>" }`

## Zachowanie

- `Authorization: Api-Key <klucz_z_grupą_create_key>`
- Nowy klucz dziedziczy organizację z klucza wywołującego
- Kolizja nazwy → `400`
- **201:** `{ "key": "<sekret>" }` — zapisz od razu; nie powtarzaj w logach

## Uwaga

To nie jest `/api/v1/create-dev-api-key` z `gaius/urls.py`.
