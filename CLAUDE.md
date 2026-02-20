# CLAUDE.md — MCP Google Sheets (fork 3KStudio)

## Cel forka
Fork z `xing5/mcp-google-sheets` (MIT license). Dodaje narzędzia specyficzne
dla workflow konferencyjnego THT, które nie pasują do upstream.

## Co zostało zmienione

### Nowy tool: `export_to_csv`
- **Commit:** `0f99633`
- **Cel:** Eksport danych arkusza bezpośrednio do CSV na dysku. Zwraca agentowi
  TYLKO metadane (path, rows, columns, bytes) — zero danych w kontekście.
- **Motywacja:** ~400 wierszy × 17 kolumn = ~76K znaków JSON w kontekście agenta.
  `export_to_csv` redukuje to do ~100 znaków metadanych.
- **Lokalizacja:** `src/mcp_google_sheets/server.py`, linie ~1392-1469

## Jak używać (z projektu THT)

W `.mcp.json` projektu THT:
```json
{
  "google-sheets": {
    "command": "uv",
    "args": [
      "run",
      "--directory", "D:\\!FilmyPanas-2026\\2026_MCP\\mcp-google-sheets",
      "mcp-google-sheets"
    ]
  }
}
```

## Wzorzec dodawania nowych narzędzi

1. W `server.py` użyj dekoratora `@tool()` (wrapper na `@mcp.tool()`
   z obsługą filtrowania via `--include-tools`)
2. Dodaj `ToolAnnotations` z `title` i `readOnlyHint`/`destructiveHint`
3. Funkcja otrzymuje `ctx: Context` — dostęp do Google API:
   - `ctx.request_context.lifespan_context.sheets_service`
   - `ctx.request_context.lifespan_context.drive_service`
4. Zwróć `Dict[str, Any]`
5. Dodaj tool do listy w README.md (sekcja Available Tools + Tool Filtering)

## Sync z upstream

Fork jest na branchu `main`. Upstream: `xing5/mcp-google-sheets`.
Sync ręcznie: GitHub → "Sync fork" lub `git fetch upstream && git merge`.
