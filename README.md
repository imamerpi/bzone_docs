# BZone Public Documentation Repository

Official multi-language documentation repository for BZone Customs & IT Inventory ecosystem.

## Structure
- `manifest.json`: Defines global settings, supported languages, default language, and branch versions.
- `id/`: Bahasa Indonesia content.
  - `toc.json`: Sidebar navigation tree for Bahasa Indonesia.
  - `index.md`: Main landing guide.
- `en/`: English content.
  - `toc.json`: Sidebar navigation tree for English.
  - `index.md`: Main landing guide.
- `assets/`: Media files, screenshots, and diagrams.
- `CHANGELOG*.md`: Release notes and version history.

## Adding a New Language
1. Create a new directory named after the ISO language code (e.g. `zh`, `ja`, `es`).
2. Add `toc.json` and markdown files inside the language directory.
3. Register the language in `manifest.json` under the `languages` array.
4. BZone Documentation portal will automatically detect and list the new language in the dropdown selector!
