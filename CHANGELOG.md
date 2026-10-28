# Changelog

Release history for this pack. Newest first.

## v0.1.0 (unreleased)

- Initial pack: `macos_powermetrics`, `macos_wired_memory`, and `macos_thermal`
  pipelines, each with a constant `index` and `sourcetype`.
- Exec command text for powermetrics and wired memory in `default/exec/`.
- Routes matching each exec input by `__inputId`.
- Fixtures per pipeline and sample events for the Cribl UI.
