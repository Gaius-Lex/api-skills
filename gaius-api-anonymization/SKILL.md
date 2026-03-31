---
name: gaius-api-anonymization
description: Documents synchronous text anonymization API. Use when integrating `POST /api/v1/anonymization`, optional pre-tagged entities, or `AnonymizationRequest` validation.
---

# Anonymization API

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/api/views/anonymization.py` · Wejście: `AnonymizationRequest` w `gaius/api/serializers.py`

## `POST /api/v1/anonymization`

- `text`, opcjonalnie `entities` (struktura po zdaniach), `threshold`, `include_lemmas`
- Odpowiedź: m.in. `anonymized_text`, `entities`, `sentence_entities`

## Autoryzacja

`Authorization: Api-Key <klucz>`
