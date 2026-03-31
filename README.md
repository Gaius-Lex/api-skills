# Gaius-Lex API skills (Cursor)

Skille są utrzymywane w strukturze zgodnej z [Cursor Skills](https://cursor.com): jeden katalog na skill, w środku `SKILL.md` z frontmatter (`name`, `description`).

**Kanoniczna lokalizacja (źródło edycji):** [`.cursor/skills/`](../../.cursor/skills/)

**Ten katalog (`misc/api-skills/`)** jest **lustrzanym kopiowaniem** tych samych plików — wrzucamy go do repozytorium obok kodu. Po zmianach w `.cursor/skills/**/SKILL.md` zsynchronizuj kopię (z katalogu `misc/api-skills/`):

```bash
rsync -a ../../.cursor/skills/ ./ --include='*/' --include='SKILL.md' --exclude='*'
```

Bez `--delete` — nadpisuje tylko `SKILL.md`, nie usuwa np. lokalnego `.git` ani `README.md`. (Z `--delete` pojawiają się konflikty, jeśli w tym katalogu jest własne repo.)

Alternatywa: skopiuj ręcznie wybrane `SKILL.md`.

- Start od indeksu: [`gaius-lex-api-catalog/SKILL.md`](gaius-lex-api-catalog/SKILL.md)

Poprzednie pojedyncze pliki `SKILL-*.md` w tym katalogu zostały zastąpione powyższą strukturą — nie duplikuj treści poza tymi dwoma drzewami.
