---
name: gaius-api-files
description: Organization external file API (vectorize upload, list, full text by fileId). Use when integrating `/api/v1/external/*`, finding text inside API-uploaded docs when search/own-database is unreliable, or `external_upload.py` flows.
---

# Files Upload (external)

**OpenAPI:** [Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Baza:** `https://api.gaius-lex.pl`

**Indeks:** [gaius-lex-api-catalog](../gaius-lex-api-catalog/SKILL.md)

Wymagany klucz organizacji (`403` jeśli brak powiązania).

**Kod:** `legalgpt/gaius/api/views/external_upload.py`

## `POST /api/v1/external/vectorize`

- `multipart/form-data`, pole `file`
- Rozszerzenia: m.in. `pdf`, `txt`, `docx`, obrazy, audio — pełna lista w `ExternalVectorizeUploadView.ALLOWED`

**Odpowiedź:** `status`, `path`, `fileId`, `already_existed` — `200` / `201`

## `GET /api/v1/external/document/<file_id>`

Treść z katalogu **external** tej organizacji. Odpowiedź m.in. `fileId`, `path`, `name`, **`content`** (pełny tekst). `404` / `409` (treść niegotowa — wektoryzacja w toku).

## `GET /api/v1/external/list-documents`

Tylko dokumenty z `VectorizationStatus.is_api=True`. Odpowiedź: `{ "documents": [ ... ] }` bez query w widoku.

## Autoryzacja

`Authorization: Api-Key <klucz>`

### Use case: w którym pliku jest dana treść (np. nazwa własna, fundacja)?

**Kontekst:** `GET /api/v1/search` z filtrem `categories=pl_own_database` („Mój dysk”) bywa nietypowy (np. brak trafień albo błąd serwera). Pliki wgrane wyłącznie przez to API leżą w **`external/`** — do nich masz gwarantowany dostęp po **`fileId`** bez polegania na wyszukiwarce ES.

**Procedura:**

1. **`GET /api/v1/external/list-documents`** — lista `fileId`, `name`, `path`, status wektoryzacji.
2. Dla podejrzanych pozycji (albo po kolei, jeśli lista jest krótka): **`GET /api/v1/external/document/<file_id>`** — przeszukaj po stronie klienta pole **`content`** (np. szukaj frazy, regex).
3. Jeśli `409`, odczekaj i powtórz (treść jeszcze nie zapisana po wektoryzacji).

**Uwaga:** To dotyczy **uploadów API** (`external`). Inne ścieżki („Mój dysk” z UI, współdzielone katalogi) nie są tym samym endpointem.

**Przeszukiwanie** (semantyczne / w jednym pliku) z **`Api-Key`**: [gaius-api-query-documents](../gaius-api-query-documents/SKILL.md).

### Use case: upload pliku → RAG (`query-documents`) i fraza w jednym pliku (`document-search`)

**Cel:** Wgrać dokument (np. OCR `*.txt`), odczekać gotowość treści / wektorów, potem zapytać semantycznie po całym `external/` albo wyszukać frazę w konkretnym pliku.

1. **`POST /api/v1/external/vectorize`** — `multipart/form-data`, pole `file` (np. `10514.ocr.txt`). Odpowiedź: `path` (np. `external/10514.ocr.txt`), `fileId`.
2. Opcjonalnie **`GET /api/v1/external/document/<file_id>`** — jeśli `409`, powtarzaj aż `200` (treść zapisana po wektoryzacji); przy krótkich plikach często od razu `200`.
3. **`GET /api/v1/external/list-documents`** — potwierdzenie `path` / statusu wektoryzacji.
4. Dalej wyłącznie **`Api-Key`**: semantyka **`POST /api/v1/query-documents`** z `path: "external/"` (lub węższy prefiks) oraz szukanie frazy **`GET /api/v1/document-search?query=...&path=external/<nazwa_pliku>`** — szczegóły i ograniczenia: [gaius-api-query-documents](../gaius-api-query-documents/SKILL.md).

## Kod

- `legalgpt/gaius/api/views/external_upload.py`
- `legalgpt/gaius/urls.py` — ścieżki `api/v1/external/*`
