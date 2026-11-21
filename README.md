# Latra Workshop Scan

Scan a Canonical Workshop project with [Latra](https://github.com/shizuku198411/Latra) on a GitHub-hosted `ubuntu-latest` runner. The action installs the `latra` snap from `latest/stable` and prints the installed version. Pinning the action tag pins the wrapper. The Latra version follows the Snap channel.

## Usage

Example for the v1.0.0 release:

```yaml
name: Workshop scan
on: [push, pull_request]
permissions:
  contents: read
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: shizuku198411/latra-actions@v1.0.0
        with:
          fail-on: 'risk>=high'
```

| Input | Default | Purpose |
| --- | --- | --- |
| `project` | `.` | Workshop project directory relative to the checkout. |
| `fail-on` | Empty | Fail when any SDK matches the condition after suppression. Omit to scan without a risk threshold. |

Place suppression rules in `.latra/suppression_rule.yaml` under the Workshop project root selected by `project`.

Use `>=` with a level (`info`, `low`, `medium`, `high`, `critical`), or `>=` and `>` with an integer from 0 to 100. Examples include `risk>=high`, `risk>=80` and `risk>90`.

The scan report appears in the step log. With a threshold, Latra also writes its verdict to the GitHub job summary. Exit code 1 covers both a matched threshold and an operational error. Consult the diagnostics for the reason. Invalid arguments return exit code 2.
