# Inkay-Switcher-Config

Source manifest for [Inkay-Switcher](https://github.com/Happynico7504/Inkay-Switcher). The app fetches
`sources.json` from this repo's `main` branch at startup to populate its version-selection list, so new
sources can be added/changed without shipping a new app release.

## Schema

```json
{
  "sources": [
    {
      "id": "unique-id",
      "name": "Display Name",
      "color": "RRGGBBAA",
      "type": "github_zip | github_raw | direct_raw",
      "repo": "owner/repo",
      "wms_url": "https://...",
      "wps_url": "https://..."
    }
  ]
}
```

- `id`, `name`, `type` are required for every entry.
- `color` is an optional `RRGGBBAA` (or `RRGGBB`, alpha defaults to `FF`) hex string used for the
  entry's text color in the app; a `0x` or `#` prefix is also accepted. Falls back to a default color
  if omitted or invalid.
- `type: "github_zip"` requires `repo`. The app resolves the latest release via the GitHub API and
  downloads the first release asset as a zip containing both the `.wms` module and `.wps` plugin.
- `type: "github_raw"` requires `repo`. The app resolves the latest release via the GitHub API and
  downloads the `.wms`/`.wps` assets directly by matching filename.
- `type: "direct_raw"` requires `wms_url` and `wps_url` pointing directly at the module/plugin files
  (skips the GitHub API lookup entirely).

Any entry missing its required fields, or with an unrecognized `type`, is skipped by the app rather
than failing the whole manifest.
