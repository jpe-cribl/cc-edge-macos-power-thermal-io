# cc-edge-macos-power-thermal-io

Cribl Edge pack for macOS power, wired memory, and thermal telemetry. It holds
three pipelines and the exec command text for the two sources that need a
multi-part command.

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

| Pipeline | Exec input id | `index` | `sourcetype` | Event content |
| --- | --- | --- | --- | --- |
| `macos_powermetrics` | `exec:in_macos_powermetrics` | `mac_perf` | `macos:perf:powermetrics` | One powermetrics sample as JSON |
| `macos_wired_memory` | `exec:in_macos_wired_memory` | `mac_perf` | `macos:perf:wired_memory` | Wired bytes, GPU wired ceiling, wired ratio |
| `macos_thermal` | `exec:in_macos_thermal` | `os` | `macos:system:thermal` | Output of `pmset -g therm` |

Each pipeline sets its own constant `index` and `sourcetype` and adds no other
fields.

The exec command text for the two multi-part sources lives in
`default/exec/`:

- `default/exec/macos-powermetrics.cmd`
- `default/exec/macos-wired-memory.cmd`

Each file holds the exact command string with no trailing newline. The
`pmset -g therm` command is a single command and has no file.

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

- Initial pack: `macos_powermetrics`, `macos_wired_memory`, and `macos_thermal`
  pipelines, with the exec command text for powermetrics and wired memory.
