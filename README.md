# Skille Gaius-Lex API

Ten katalog to **gotowe opisy** poszczególnych obszarów [API Gaius-Lex](https://api.gaius-lex.pl): jakie są endpointy, parametry, autoryzacja i typowe sposoby użycia. Możesz z nich korzystać **samodzielnie** (czytasz pliki `SKILL.md`) albo **w Cursorze** — wtedy asystent ma kontekst przy pisaniu integracji.

**Formalna specyfikacja:** [Gaius-Lex API 2.0 (OpenAPI)](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en)  
**Środowisko produkcyjne:** `https://api.gaius-lex.pl`

## Czego będziesz potrzebować

- **Klucz API organizacji** (w nagłówku `Authorization: Api-Key …` tam, gdzie skill to opisuje).
- Dostęp do dokumentacji wybranego obszaru — zacznij od indeksu poniżej.

## Jak zacząć

1. Otwórz **[indeks — `gaius-lex-api-catalog/SKILL.md`](gaius-lex-api-catalog/SKILL.md)** — zobaczysz tabelę: który plik dotyczy którego typu funkcji (wyszukiwanie, pliki, agent, KRS itd.).
2. Wejdź w wybrany **`…/SKILL.md`** — tam są konkretne ścieżki HTTP, pola i uwagi (np. limity, BETA).
3. Dopasuj to do OpenAPI — skill ma skrócić orientację, Swagger ma pełne schematy.

**Przykłady na start:** wyszukiwanie i pobieranie treści po `gLexID` → [`gaius-api-search/SKILL.md`](gaius-api-search/SKILL.md); wgrywanie i odczyt plików → [`gaius-api-files/SKILL.md`](gaius-api-files/SKILL.md); KRS → [`gaius-api-krs/SKILL.md`](gaius-api-krs/SKILL.md).

## Proste scenariusze

| Chcę… | Skill |
|--------|--------|
| Zadać **złożone pytanie prawne** z narzędziami i źródłami (asynchronicznie) | [`gaius-api-agent`](gaius-api-agent/SKILL.md) |
| Znaleźć akt lub orzeczenie po tytule / sygnaturze, potem pobrać **pełny tekst** z bazy | [`gaius-api-search`](gaius-api-search/SKILL.md) (`/search` → `gLexID` → `/document-glexid`) |
| **Wgrać plik** (PDF, TXT, DOCX…), potem **odczytać treść** lub znaleźć frazę we wgranych dokumentach | [`gaius-api-files`](gaius-api-files/SKILL.md) |
| Dostać **jedną odpowiedź** na pytanie (research, bez pełnego agenta) | [`gaius-api-answer`](gaius-api-answer/SKILL.md) |
| Przetworzyć skan **OCR** (obraz/PDF → tekst) | [`gaius-api-ocr`](gaius-api-ocr/SKILL.md) |
| Sprawdzić **KRS** (powiązania, zarząd, umowa spółki) | [`gaius-api-krs`](gaius-api-krs/SKILL.md) |
| **Zanonimizować** tekst przed dalszym użyciem | [`gaius-api-anonymization`](gaius-api-anonymization/SKILL.md) |
| Uruchomić **weryfikację dokumentu** (workflow z pollingiem) | [`gaius-api-doc-verification`](gaius-api-doc-verification/SKILL.md) |
| Analiza **abusivity** / **tez** / **reguł** (funkcje BETA) | [`gaius-api-abusivity`](gaius-api-abusivity/SKILL.md), [`gaius-api-theses`](gaius-api-theses/SKILL.md), [`gaius-api-rule-analysis`](gaius-api-rule-analysis/SKILL.md) |

## Cursor

Jeśli pracujesz w **Cursorze**, możesz podpiąć ten folder jako [skills](https://docs.cursor.com/context/skills) — wtedy w czacie łatwiej odwołać się do konkretnego API (np. „według gaius-api-search…”).

## Spis skilli

| Skill | Temat |
|-------|--------|
| [gaius-lex-api-catalog](gaius-lex-api-catalog/SKILL.md) | Indeks wszystkich |
| [gaius-api-agent](gaius-api-agent/SKILL.md) | Agent, profile, polling |
| [gaius-api-search](gaius-api-search/SKILL.md) | Wyszukiwanie, źródła, dokument po `gLexID` |
| [gaius-api-files](gaius-api-files/SKILL.md) | Pliki `external`, lista, treść po `fileId` |
| [gaius-api-answer](gaius-api-answer/SKILL.md) | Answer API |
| [gaius-api-ocr](gaius-api-ocr/SKILL.md) | OCR |
| [gaius-api-anonymization](gaius-api-anonymization/SKILL.md) | Anonimizacja |
| [gaius-api-doc-verification](gaius-api-doc-verification/SKILL.md) | Weryfikacja dokumentów |
| [gaius-api-abusivity](gaius-api-abusivity/SKILL.md) | Abusivity (BETA) |
| [gaius-api-theses](gaius-api-theses/SKILL.md) | Tezy (BETA) |
| [gaius-api-rule-analysis](gaius-api-rule-analysis/SKILL.md) | Analiza reguł (BETA) |
| [gaius-api-krs](gaius-api-krs/SKILL.md) | KRS (BETA) |

Pełniejsza tabela po angielsku — w **[gaius-lex-api-catalog/SKILL.md](gaius-lex-api-catalog/SKILL.md)**.

---

Sekcje **„Kod”** w niektórych skillach to tylko skrót do modułów backendu — **do integracji HTTP nie są potrzebne**; możesz je zignorować.
