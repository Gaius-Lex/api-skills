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

**Uwaga:** To dotyczy **uploadów API** (`external`). Inne ścieżki („Mój dysk” z UI, współdzielone katalogi) nie są tym samym endpointem — tam typowe jest np. `POST /api/v1/query-documents` (sesja), a nie ten skill.

## Kod

- `legalgpt/gaius/api/views/external_upload.py`
- `legalgpt/gaius/urls.py` — ścieżki `api/v1/external/*`
