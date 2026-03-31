---
name: gaius-api-krs
description: KRS HTTP endpoints for company relationships, agreement text, and board/shareholder lookups. Use when integrating `/api/v1/krs`, `/api/v1/krs-agreement`, text analysis, or API-key auth from `krs.py`.
---

# KRS API (BETA)

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Kod:** `legalgpt/gaius/views/krs.py`

## Autoryzacja

Sesja **lub** `ApiKeyAuthentication` + `HasActiveOrganizationApiKey`. Przy samym UI mogą obowiązywać limity subskrypcji.

## `GET /api/v1/krs`

Query: **`krs`** (numer KRS).

Zwraca uproszczone powiązania (m.in. wspólnicy, zarząd); mocki w dev/prod wg flag.

### Use case: kto jest w zarządzie

Odpowiedź zawiera tablicę **`board`**: elementy z polami w stylu **`role`** (np. prezes, członek zarządu) i **`name`**. Numer KRS podawaj z zerami wiodącymi zgodnie z oczekiwanym formatem (np. `0001047596`).

## `POST /api/v1/krs-text-analysis`

Body: `text` — `filePath` na razie nieobsługiwane.

## `GET /api/v1/krs-agreement`

Query **`krs`** — reprezentacja / treść umowy (upstream może zwrócić błąd, np. 404 od dostawcy).

## Throttling

`APIKeyRateThrottle`
