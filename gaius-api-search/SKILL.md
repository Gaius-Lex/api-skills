---
name: gaius-api-search
description: Full-text search, available-sources, and full judiciary/corpus documents by gLexID (search then document-glexid). Use when integrating `/api/v1/search`, resolving case signatures to hits, or `GET /api/v1/document-glexid` with an organization API key.
---

# Search API, źródła i pełny dokument po gLexID

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

Schematy EchoAPI mogą używać placeholderów — parametry poniżej odpowiadają `WebSearchView`, `DocumentGlexidView` i `AvailableSourcesView` w `gaius/views/views.py`. Logika fragmentów wyników: `gaius/workflows/search_bar.py`.

## Autoryzacja

- `Authorization: Api-Key <klucz>` **albo** sesja (UI).
- Dla `search`: brak nagłówka → `401` (chyba że zalogowany użytkownik).
- **`document-glexid`:** ten sam wzorzec co KRS — sesja **lub** `Api-Key` + aktywna organizacja (`HasActiveOrganizationApiKey`).

## `GET /api/v1/available-sources`

Mapa `internal_name → nazwa` źródeł (domyślnie PL; query: `country`). Przy braku sesji wymagany `Api-Key`.

## `GET /api/v2/available-sources`

`AvailableSourcesView2` — parametry m.in. `country` / `contry` (literówka w query).

## `GET /api/v1/search`

| Parametr | Znaczenie |
|----------|-----------|
| `q` | Zapytanie (wymagane) |
| `page`, `page_size` | Paginacja (domyślnie 1 / 10) |
| `sort_by` | `score` \| `date-asc` \| `date-desc` |
| `date_from`, `date_to` | Filtr dat |
| `categories` | Powtarzalne (np. `pl_judiciary` = orzecznictwo) |
| `language` | Domyślnie `pl` |

Dodatkowe filtry: `SearchFilters.from_request(request)`.

**Odpowiedź:** `result`, `pagination_info`, `shortcut_response`, `shortcut_document_url`.

Każdy element `result` (poza wpisami `webcrawl`) ma m.in. `headline`, **`highlight`** (fragmenty dopasowania), `source`, `date`, `extra_info`, **`gLexID`** (gdy dotyczy dokumentu w indeksie).

### Use case: sygnatura orzeczenia (np. SN)

1. **Wyszukiwanie po dokładnej sygnaturze:** ustaw `q` w cudzysłowie, np. `q="III CZP 7/25"`, żeby uniknąć ścieżki „tylko sygnatura” bez dopasowania w polu `text` (patrz `detect_isonly_orzeczenie` w `search_bar.py`).
2. **Identyfikacja:** z pierwszego trafienia weź `gLexID` (oraz metadane z `extra_info`: typ orzeczenia, sąd, skład itd.).
3. **Pełna treść z API:** pole **`highlight` nie jest pełnym tekstem** — to sklejone fragmenty z Elasticsearch (łączone `(...) `). Pełne źródłowe dane (w tym pole `text`) pobierz osobnym requestem (poniżej).

### Throttling (`search`)

`APIKeyDailyRateThrottle` — limit dzienny per klucz przy obecnym nagłówku `Authorization` (implementacja: `gaius/rate_limit.py`).

## `GET /api/v1/document-glexid`

Query: **`gLexID`** (wymagane).

**Odpowiedź:** `{ "status": "ok", "content": <dict z Elasticsearch> }` — m.in. pełny tekst w `content["text"]` (oraz pozostałe pola dokumentu).

**Kiedy używać:** po `search`, gdy potrzebujesz całego tekstu orzeczenia/aktu, a nie samego snippetu.

**Throttling:** `APIKeyRateThrottle` (per klucz z nagłówkiem `Authorization`).

## Kod

- `legalgpt/gaius/views/views.py` — `WebSearchView`, `DocumentGlexidView`, `AvailableSourcesView`, `AvailableSourcesView2`
- `legalgpt/gaius/workflows/search_bar.py` — zapytanie ES, `highlight` → `text` w wyniku
- `legalgpt/gaius/urls.py`
