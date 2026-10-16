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
- `c_zh/`: 简体中文 (Simplified Chinese) content.
  - `toc.json`: Sidebar navigation tree for Simplified Chinese.
  - `index.md`: Main landing guide.
- `assets/`: Media files, screenshots, and diagrams.
- `CHANGELOG*.md`: Release notes and version history (per language).

## Adding a New Language
1. Create a new directory named after the language code (e.g. `zh`, `ja`, `es`, `c_zh`).
2. Add `toc.json` and markdown files inside the language directory.
3. Register the language in `manifest.json` under the `languages` array.
4. Optionally add a `CHANGELOG_<code>.md` for localized change history.
5. BZone Documentation portal will automatically detect and list the new language in the dropdown selector!

## Currently Supported Languages
| Code | Name | Default |
| :--- | :--- | :--- |
| `id` | Bahasa Indonesia | ✅ |
| `en` | English | |
| `c_zh` | 简体中文 (Simplified Chinese) | |
