# VEXLIT Security Scan Action

> Automated security scanning for GitHub pull requests and CI/CD pipelines.

## Usage

```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: vexlit/action@v1
        with:
          format: sarif
          output: results.sarif
          fail-on: high
      - name: Upload SARIF
        if: always()
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: results.sarif
```

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `format` | Output format: `table`, `json`, `sarif` | `sarif` |
| `output` | Output file path | `results.sarif` |
| `fail-on` | Fail on severity: `critical`, `high`, `medium`, `low` | `critical` |
| `path` | Directory to scan | `.` |

## Outputs

| Output | Description |
|--------|-------------|
| `exit-code` | CLI exit code: `0` = clean, `1` = vulnerabilities found, `2` = error |
| `sarif-file` | Path to the SARIF output file |

## Exit Codes

| Code | Meaning | Action |
|------|---------|--------|
| `0` | No vulnerabilities above threshold | Workflow continues |
| `1` | Vulnerabilities found above `fail-on` threshold | Workflow fails with summary |
| `2` | Scan error (e.g. invalid arguments) | Workflow fails with error details |

When vulnerabilities are found, the action prints a clear summary:

```
============================================================
❌ VEXLIT: Security vulnerabilities detected
   Found: 3 vulnerabilities (1 Critical, 1 High)
   Threshold: --fail-on critical
   Results: results.sarif
============================================================
```

## Features

- **Clear Exit Summaries** — Human-readable messages explain why the scan passed or failed
- **Automatic PR Annotations** — Vulnerability findings appear as inline comments on pull requests
- **SARIF Upload** — Results integrate with GitHub's Security tab
- **Configurable Severity Gates** — Block merges on critical vulnerabilities
- **6,200+ Security Rules** — Covering OWASP Top 10 across 34 languages
- **Fast** — Average scan time under 3 seconds

## Examples

### Block PRs with Critical Vulnerabilities

```yaml
- uses: vexlit/action@v1
  with:
    fail-on: critical
```

### Scan Specific Directory

```yaml
- uses: vexlit/action@v1
  with:
    path: src/
```

### Continue Workflow After Scan

```yaml
- uses: vexlit/action@v1
  id: scan
  continue-on-error: true

- name: Handle Results
  if: steps.scan.outputs.exit-code == '1'
  run: echo "Vulnerabilities found — review results.sarif"
```

## Documentation

Full documentation at [vexlit.ai/docs](https://vexlit.ai/docs)

## License

MIT
