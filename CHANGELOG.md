# Changelog

Release history for this pack. Newest first.

## v0.2.0

- `macos_battery` pipeline: index `os`, sourcetype `macos:power:battery`, a
  `battery_health_percent` field and an anomaly flag below 80 percent health.
- `macos_macmon` pipeline: index `mac_perf`, sourcetype `macos:perf:macmon`,
  flattened temperature, memory and GPU fields, and 13 gauge metrics.
- Exec command text for thermal, battery and macmon in `default/exec/`, with
  routes by exec input id, fixtures and sample events.

## v0.1.0

- Initial pack: `macos_powermetrics`, `macos_wired_memory`, and `macos_thermal`
  pipelines, each with a constant `index` and `sourcetype`.
- Exec command text for powermetrics and wired memory in `default/exec/`.
- Routes matching each exec input by `__inputId`.
- Fixtures per pipeline and sample events for the Cribl UI.
