<div align="center">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64" width="128" height="128">
    <!-- Document with styled layers — represents multi-style report generation -->
    <!-- Back layer (neon) -->
    <rect x="16" y="4" width="36" height="44" rx="2" fill="#0a0a1a" stroke="#00f0ff" stroke-width="1"/>
    <rect x="20" y="8" width="12" height="2" fill="#00f0ff"/>
    <rect x="20" y="12" width="28" height="1" fill="#1a1a3a"/>
    <rect x="20" y="15" width="28" height="1" fill="#1a1a3a"/>
    <rect x="20" y="18" width="20" height="1" fill="#1a1a3a"/>
    <!-- Middle layer (notion) -->
    <rect x="10" y="10" width="36" height="44" rx="2" fill="#fff" stroke="#e0e0e0" stroke-width="1"/>
    <rect x="14" y="14" width="14" height="2" fill="#333"/>
    <rect x="14" y="18" width="28" height="1" fill="#eee"/>
    <rect x="14" y="21" width="28" height="1" fill="#eee"/>
    <rect x="14" y="24" width="22" height="1" fill="#eee"/>
    <!-- Front layer (terminal) -->
    <rect x="4" y="16" width="36" height="44" rx="2" fill="#0a0a0a" stroke="#33ff33" stroke-width="1"/>
    <rect x="4" y="16" width="36" height="6" rx="2" fill="#1a1a1a"/>
    <circle cx="8" cy="19" r="1.5" fill="#ff5f56"/>
    <circle cx="13" cy="19" r="1.5" fill="#ffbd2e"/>
    <circle cx="18" cy="19" r="1.5" fill="#27c93f"/>
    <rect x="8" y="26" width="6" height="2" fill="#33ff33"/>
    <rect x="16" y="26" width="18" height="2" fill="#33ff33" opacity="0.5"/>
    <rect x="8" y="30" width="24" height="1" fill="#33ff33" opacity="0.3"/>
    <rect x="8" y="33" width="20" height="1" fill="#33ff33" opacity="0.3"/>
    <rect x="8" y="36" width="28" height="1" fill="#33ff33" opacity="0.3"/>
    <!-- Style switcher arrow -->
    <polygon points="48,36 56,42 48,48" fill="#3b82f6" opacity="0.8"/>
    <polygon points="50,38 55,42 50,46" fill="#60a5fa"/>
  </svg>
</div>

<h1 align="center">claude-reports</h1>

<p align="center">
  Markdown-to-HTML report generator with 13 visual styles — self-contained, zero-dependency output.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Node.js-339933?logo=node.js&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/styles-13-blue" alt="13 Styles" />
  <img src="https://img.shields.io/badge/output-self--contained_HTML-green" alt="Self-contained" />
</p>

---

## About

**claude-reports** converts structured JSON (parsed from markdown) into polished, self-contained HTML reports. Each generated file is a single `index.html` with all CSS and JS inlined — no external local dependencies, works offline, can be emailed or shared as-is.

The architecture separates **content parsing** (LLM or script produces JSON) from **rendering** (TypeScript generator outputs styled HTML). This means you can restyle any report without re-parsing the source data — just run the restyle script with a different style name.

Includes 13 production-ready visual styles covering everything from clean reading layouts to cyberpunk neon dashboards, plus a standalone interactive table generator.

## Quick Start

```bash
# Clone
git clone https://github.com/alcatraz627/claude-reports.git
cd claude-reports

# Install
npm install

# Generate a report from JSON data
npx tsx generate-html.ts data.json output/index.html --style minimal

# Restyle an existing report (no re-parsing needed)
bash restyle-report.sh output/ neon
```

## How It Works

```
┌──────────────┐      ┌──────────────┐      ┌──────────────────┐
│   Markdown   │─────▶│  Structured  │─────▶│  Self-Contained  │
│   (source)   │ parse│    JSON      │render │    HTML Report   │
└──────────────┘      │  data.json   │      │   (CSS+JS inline)│
                      └──────┬───────┘      └──────────────────┘
                             │ restyle
                      ┌──────▼───────┐
                      │  Same JSON,  │
                      │ new style    │──▶ Different HTML
                      └──────────────┘
```

### Input JSON Schema

The generator expects a JSON file with this structure:

```json
{
  "title": "Report Title",
  "subtitle": "Optional subtitle",
  "generated": "2026-04-18",
  "nav": [
    { "id": "section-slug", "text": "Section Name", "level": 2 }
  ],
  "sections": [
    {
      "id": "section-slug",
      "heading": "Section Name",
      "level": 2,
      "blocks": [
        { "type": "paragraph", "html": "Text with <strong>inline</strong> HTML" },
        { "type": "code", "lang": "typescript", "content": "const x = 1;" },
        { "type": "table", "headers": ["Col1", "Col2"], "rows": [["a", "b"]] },
        { "type": "ul", "items": ["Item 1", "Item 2"] },
        { "type": "tree", "nodes": [{ "label": "src/", "children": [...] }] },
        { "type": "math", "latex": "E = mc^2", "display": true }
      ]
    }
  ]
}
```

**Supported block types:** `paragraph`, `code`, `table`, `ul`, `ol`, `blockquote`, `hr`, `math`, `tree`, `subsection`, `subsubsection`

## Styles

| Style | Description |
|-------|-------------|
| `default` | Dark sidebar report with accent colors, font/color/width pickers |
| `minimal` | Ultra-clean reading-focused layout with maximum whitespace |
| `notion` | Clean, minimal Notion-style with cards and whitespace |
| `dashboard` | Dark analytics dashboard with metric cards and status pills |
| `data-table` | Data-heavy spreadsheet layout optimized for tables |
| `neon` | Cyberpunk neon-glow dark theme with 18 accent color presets |
| `terminal` | Green-on-black retro terminal with CRT scanlines |
| `magazine` | Editorial magazine layout with serif typography and hero header |
| `jupyter` | Jupyter notebook with executable-style cells |
| `academic` | LaTeX-inspired academic paper with serif fonts and numbered sections |
| `corporate` | Formal corporate/legal report — print-ready, numbered sections |
| `feed` | Social feed layout for narrative data — timeline cards |
| `slide` | Presentation-style with full-viewport sections and arrow key navigation |

Every style includes:
- **Dark/light mode toggle** with localStorage persistence
- **Floating toolbar** with print, theme, and style picker
- **Syntax highlighting** for code blocks (20+ languages)
- **KaTeX math rendering** for LaTeX expressions
- **Search** across all sections
- **Responsive layout** for mobile/tablet/desktop

## Usage

### Generate a Single Style

```bash
npx tsx generate-html.ts <input.json> <output.html> --style <style_name>
```

### Generate All 13 Styles at Once

```bash
npx tsx generate-html.ts data.json output/ --all-styles
```

This creates `output/<style>/index.html` for each style plus a launcher page at `output/index.html`.

### Restyle Without Re-Parsing

Every generated report includes a `data.json` alongside the HTML. To switch styles:

```bash
bash restyle-report.sh <report_dir> <new_style>
```

### List Available Styles

```bash
bash scripts/list-styles.sh
```

### Interactive Table Generator

For tabular data (CSV/JSON), use the dedicated table generator:

```bash
npx tsx table/generate-table.ts data.json output.html \
  --title "Sales Report" \
  --search --pagination 25 --striped \
  --sort-by "Revenue:desc" --export csv
```

See [table/USAGE.md](table/USAGE.md) for the full flag reference.

## Project Structure

```
claude-reports/
├── generate-html.ts        Core HTML generator (1584 lines)
├── report.js               Default template client-side JS
├── shared.js               Shared JS modules (toolbar, search, copy, math)
├── shared-base.css          Shared base CSS (reset, variables, toolbar)
├── styles.css              Default template styles
├── restyle-report.sh       Zero-re-parse style switcher
├── package.json            Dependencies: tsx, vitest, playwright
├── scripts/
│   └── list-styles.sh      List all available styles
├── styles/
│   ├── academic/           Each style directory contains:
│   ├── corporate/            ├── meta.json      Style metadata
│   ├── dashboard/            ├── template.ts    HTML template
│   ├── data-table/           ├── style.css      Style-specific CSS
│   ├── feed/                 └── style.js       Style-specific JS
│   ├── jupyter/
│   ├── magazine/
│   ├── minimal/
│   ├── neon/
│   ├── notion/
│   ├── slide/
│   └── terminal/
├── table/
│   ├── generate-table.ts   Standalone table generator
│   └── USAGE.md            Table generator documentation
└── tests/
    ├── fixture.json        Test data fixture
    ├── static.test.ts      88 static HTML generation tests
    └── toolbar.spec.ts     Playwright browser interaction tests
```

## Testing

```bash
# Run static tests (no browser needed)
npm test

# Run Playwright browser tests
npx playwright test tests/toolbar.spec.ts
```

The static test suite validates all 13 styles for:
- Self-contained output (no external CSS/JS references)
- Floating toolbar presence and structure
- Theme toggle mechanism
- Content rendering accuracy

## Adding a New Style

1. Create a directory under `styles/<name>/`
2. Add four files:

   - **`meta.json`** — name, description, emoji, color
   - **`template.ts`** — exports `buildHtml(data: ReportData): string`
   - **`style.css`** — all CSS for the style
   - **`style.js`** — client-side interactivity, registers `window.__RPT_TOOLBAR`

3. The template imports shared helpers from `generate-html.ts`:
   ```typescript
   import type { ReportData } from "../../generate-html.ts";
   import { esc, renderBlock, renderSection, renderNav, computeStats } from "../../generate-html.ts";
   ```

4. Register `window.__RPT_TOOLBAR` with at least a `print` function:
   ```javascript
   window.__RPT_TOOLBAR = {
     print: function() { window.print(); },
     row2Html: '',
     row2Init: function() {},
   };
   ```

## Contributing

Contributions are welcome! Please open an issue or pull request.

## License

This project is open source.
