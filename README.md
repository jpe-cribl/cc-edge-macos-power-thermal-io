# cc-edge-macos-power-thermal-io

Cribl Edge pack for macOS power, battery, macmon, wired memory, and thermal
telemetry. It holds five pipelines and the exec command text for their five
exec sources.

## Installation

Build the `.crbl` archive from a checkout:

```sh
REPO_NAME=cc-edge-macos-power-thermal-io TAG_NAME=v0.1.0 ./scripts/build-crbl.sh
```

The script writes `/tmp/cc-edge-macos-power-thermal-io-v0.1.0.crbl`. Upload the
archive to Cribl Edge as a pack, from the Cribl UI or the packs API.

## Usage

Each exec source in the Edge deployment connects to one pipeline by pipeline
id. The pack's routes match a source by `__inputId`, using the input ids below.

- `macos_powermetrics`: exec input `exec:in_macos_powermetrics`, index
  `mac_perf`, sourcetype `macos:perf:powermetrics`. One powermetrics sample as
  JSON.
- `macos_wired_memory`: exec input `exec:in_macos_wired_memory`, index
  `mac_perf`, sourcetype `macos:perf:wired_memory`. Wired bytes, the GPU wired
  ceiling, and the wired ratio.
- `macos_thermal`: exec input `exec:in_macos_thermal`, index `os`, sourcetype
  `macos:system:thermal`. The output of `pmset -g therm`.
- `macos_battery`: exec input `exec:in_macos_battery`, index `os`, sourcetype
  `macos:power:battery`. Battery state, charge, cycle count, capacity, and
  temperature from pmset and ioreg, plus the derived battery health percent.
- `macos_macmon`: exec input `exec:in_macos_macmon`, index `mac_perf`,
  sourcetype `macos:perf:macmon`. GPU frequency and utilization, per-rail
  power, temperatures, and RAM, published as metrics.

Each pipeline sets its own constant `index` and `sourcetype`. The battery
pipeline also adds `battery_health_percent`, and below 80 percent it adds
`anomaly` and `anomaly_reason`. The macmon pipeline flattens its nested
temperature, memory, and GPU fields, and publishes them as metrics.

The exec command text for every exec source lives in `default/exec/`:

- `default/exec/macos-powermetrics.cmd`
- `default/exec/macos-wired-memory.cmd`
- `default/exec/macos-thermal.cmd`
- `default/exec/macos-battery.cmd`
- `default/exec/macos-macmon.cmd`

Each file holds the exact command string with no trailing newline.

powermetrics requires root.

## Testing

Fixtures sit in `tests/fixtures/<pipeline>/`. Each `<case>.json` is an input
event array, and an optional `<case>.expected.json` is a partial-match
expectation for the output.

The Vitest harness runs the pipelines against a Cribl instance on
`localhost:9000` (admin/admin). The CI workflow starts one as a service
container.

```sh
cd tests && pnpm install && pnpm run test
```

The structure validator needs `jq` and `yq`:

```sh
PACK_TYPE=edge ./scripts/validate-pack-structure.sh
```

## Contributing

Open a pull request against `main`. The CI gate and the pack test workflow must
pass before merge.

## License

No license file has been added to this repository yet.

## Release Notes

### v0.1.0 (unreleased)

- Initial pack: `macos_powermetrics`, `macos_wired_memory`, `macos_thermal`,
  `macos_battery`, and `macos_macmon` pipelines, with the exec command text for
  each exec source.
