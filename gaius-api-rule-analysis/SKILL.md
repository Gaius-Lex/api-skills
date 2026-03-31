---
name: gaius-api-rule-analysis
description: Documents rule-analysis (BETA) poll API and related CRUD routes. Use when integrating contract checks, `special_rule_ids`, progress payloads, or debugging missing DB tables for rule models.
---

# Rule Analysis API (BETA)

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/api/views/rule_analysis.py`; CRUD: `gaius/views/views.py`, `gaius/urls.py`

## `POST /api/v1/rule-analysis/poll`

- `text` (wymagany)
- `rules`, `special_rule_ids`, `contract_party_id`
- Co najmniej jedna reguła (zwykła lub specjalna); złe ID reguły specjalnej → `404`

**202:** `request_id`

## `GET /api/v1/rule-analysis/poll/<request_id>`

Postęp: `_build_progress_payload` (`progress_percent`, `results`, …)

## Inne ścieżki

`rule-analysis`, `rules-lists`, `special-rules`, `suggest-rules` — wymagają migracji; przy `UndefinedTable` uruchom migracje.

## Autoryzacja

`Authorization: Api-Key <klucz>`
