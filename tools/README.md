# Website tooling

## CSO Spillcast pages (`build_spillcast_pages.py`)

Generates the pages under `/cso_spillcast/` from the forecast `manifest.json`
published to S3 by the [cso_lstm](https://github.com/AlexLipp) forecast
deployment.

### What it touches

| Path | Generated? |
|---|---|
| `_data/cso_spillcast.json` | ✅ generated (TOC table data) |
| `_pages/cso_spillcast/{slug}.md` | ✅ generated (one stub per CSO) |
| `_pages/cso_spillcast.md` | ✋ hand-maintained (TOC page) |
| `_includes/spillcast-page.html` | ✋ hand-maintained (per-CSO template) |

### When to run

Only when the set of forecast CSOs changes, or after the model is retrained
(so reliability metrics update). The forecast **images and CSVs** update
automatically every 6 hours via S3 — no site rebuild needed for those.

```bash
python tools/build_spillcast_pages.py
git add _data/cso_spillcast.json _pages/cso_spillcast _pages/cso_spillcast.md
git commit -m "Update CSO Spillcast pages"
git push
```

The manifest URL defaults to the S3 location and matches
`spillcast_manifest_url` in `_config.yml`. Override for testing with
`--manifest-url /path/to/manifest.json`.
