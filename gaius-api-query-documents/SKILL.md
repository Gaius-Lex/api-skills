---
name: gaius-api-query-documents
description: Semantic search across own-database documents and substring search in one document, with session or Organization API key limited to external/ API uploads. Use when integrating POST /api/v1/query-documents, GET /api/v1/document-search, or RAG over files from /api/v1/external/vectorize.
---

# Own database — query i przeszukiwanie dokumentu

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

**Upload plików:** [gaius-api-files](../gaius-api-files/SKILL.md) (`external/`).

### Use case: dokument wgrany przez `external/vectorize` → pytania i szukanie frazy

Typowy flow po uploadzie z [gaius-api-files](../gaius-api-files/SKILL.md):

1. Z odpowiedzi uploadu weź `path` (np. `external/10514.ocr.txt`) i ewentualnie `fileId`.
2. **`POST /api/v1/query-documents`** — JSON: `{"text": "<pytanie>", "path": "external/"}` (lub węższy prefiks zgodny z `path` pliku). Odpowiedź: m.in. `output_text`, źródła z metadanymi wskazującymi plik pod `external/`.
3. **`GET /api/v1/document-search?query=<fraza>&path=external/<nazwa_pliku>`** — dopasowania substringów w jednym dokumencie (bez `owner` przy `Api-Key`).

Jeśli wcześniej nie było dokumentów w `external/`, `query-documents` może zwrócić 404 („dokument nie został odnaleziony”) do czasu zindeksowania; po wektoryzacji i trafieniach w Qdrant zwraca normalną odpowiedź RAG.

## Autoryzacja

Sesja **lub** `Authorization: Api-Key <klucz>` + aktywna organizacja (`HasActiveOrganizationApiKey`).

Przy **`Api-Key`** dostęp jest **tylko** do plików pod **`//<user>/external/…`** (uploady API). Inne ścieżki → **403**.

**Throttling:** `APIKeyRateThrottle` (przy nagłówku `Authorization`).

## `POST /api/v1/query-documents`

Wektorowe wyszukanie fragmentów w **wielu** dokumentach + odpowiedź z odniesieniami (`output_text`, `new_format`).

| Pole (JSON) | Znaczenie |
|-------------|-----------|
| `text` | Zapytanie (wymagane) |
| `owner` | Przy **sesji**: email / właściciel. Przy **`Api-Key`**: **pomijaj** |
| `path` | Prefiks katalogu. Przy **`Api-Key`** domyślnie **`external/`** |

## `GET /api/v1/document-search`

Szukanie frazy w **jednym** dokumencie (dopasowanie fragmentów).

| Query | Znaczenie |
|-------|-----------|
| `query` | Fraza (wymagane) |
| `path` | Ścieżka pliku w **UserTexts**. `owner` przy **`Api-Key`** można **pominąć** |
| `gLexID` | Tekst z Elasticsearch (orzecznictwo itd.) — **bez** ograniczenia `external/` |
| `docId` + `source` | Np. `pl_own_database` — przy **`Api-Key`** pełna ścieżka musi być w **`external/`** |

## Kod

- `legalgpt/gaius/views/views.py` — `QueryDocumentsView`, `SearchThroughDocument`
- `legalgpt/gaius/urls.py`
