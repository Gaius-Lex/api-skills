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

### Przykładowy request (wykrywanie encji przez serwis)

Bez `entities` backend woła zewnętrzny anonymizer; `threshold` i `include_lemmas` są opcjonalne (domyślnie `0.3` i `true`).

```bash
curl -sS -X POST 'https://api.gaius-lex.pl/api/v1/anonymization' \
  -H 'Authorization: Api-Key YOUR_ORG_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{
    "text": "Maciej mieszka przy ul. Przykładowej 123.",
    "threshold": 0.3,
    "include_lemmas": true
  }'
```

Minimalnie wystarczy sam tekst:

```bash
curl -sS -X POST 'https://api.gaius-lex.pl/api/v1/anonymization' \
  -H 'Authorization: Api-Key YOUR_ORG_API_KEY' \
  -H 'Content-Type: application/json' \
  -d '{"text":"Maciej mieszka przy ul. Przykładowej 123."}'
```

### Przykładowy request (własne encje po zdaniach)

Gdy podasz `entities`, każdy element musi mieć `sentence` oraz `entities` (lista obiektów z `text`, `label`, `start`, `end`; `confidence` opcjonalnie).

```json
{
  "text": "Maciej mieszka przy ul. Przykładowej 123.",
  "entities": [
    {
      "sentence": "Maciej mieszka przy ul. Przykładowej 123.",
      "entities": [
        { "text": "Maciej", "label": "Osoba", "start": 0, "end": 6, "confidence": 0.95 },
        { "text": "ul. Przykładowej 123", "label": "Adres", "start": 20, "end": 40, "confidence": 0.87 }
      ]
    }
  ]
}
```

## Autoryzacja

`Authorization: Api-Key <klucz>`
