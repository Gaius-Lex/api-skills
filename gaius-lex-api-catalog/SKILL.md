---
name: gaius-lex-api-catalog
description: Index of Gaius-Lex HTTP API Cursor skills (OpenAPI-backed). Use when choosing which API area applies, onboarding integrations, or locating the right skill for search, files, OCR, KRS, etc.
---

# Gaius-Lex API — skill index

**Specification:** [EchoAPI OpenAPI — Gaius-Lex API 2.0](https://openapi.echoapi.com/swagger/v3/5f57a7697802000?locale=en) · **Production base:** `https://api.gaius-lex.pl`

| Skill folder | Scope |
|----------------|--------|
| [gaius-api-agent](../gaius-api-agent/SKILL.md) | Workflow agent: `/api/v1/agent`, profiles, polling |
| [gaius-api-search](../gaius-api-search/SKILL.md) | Search, available-sources, pełny dokument po `gLexID` (`document-glexid`) |
| [gaius-api-files](../gaius-api-files/SKILL.md) | External upload, document text, list |
| [gaius-api-answer](../gaius-api-answer/SKILL.md) | Research Answer API (webhook + poll) |
| [gaius-api-ocr](../gaius-api-ocr/SKILL.md) | OCR upload + poll |
| [gaius-api-anonymization](../gaius-api-anonymization/SKILL.md) | Text anonymization |
| [gaius-api-doc-verification](../gaius-api-doc-verification/SKILL.md) | Doc verification (poll) |
| [gaius-api-abusivity](../gaius-api-abusivity/SKILL.md) | Abusivity (BETA) |
| [gaius-api-theses](../gaius-api-theses/SKILL.md) | Theses analysis (BETA) |
| [gaius-api-rule-analysis](../gaius-api-rule-analysis/SKILL.md) | Rule analysis (BETA) |
| [gaius-api-krs](../gaius-api-krs/SKILL.md) | KRS endpoints (BETA) |
| [gaius-api-core-api-key](../gaius-api-core-api-key/SKILL.md) | `POST /core_api/api-key/create` |

**Package root:** this directory tree (the published skills repo). Optional **Kod** paths in each skill point at the internal Gaius-Lex backend codebase—not required for HTTP integration.
