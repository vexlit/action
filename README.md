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
| `fail-on` | Fail on severity: `critical`, `high`, `medium`, `low` | _(none)_ |
| `path` | Directory to scan | `.` |

## Features

- **Automatic PR Annotations** — Vulnerability findings appear as inline comments on pull requests
- **SARIF Upload** — Results integrate with GitHub's Security tab
- **Configurable Severity Gates** — Block merges on critical vulnerabilities
- **490+ Security Rules** — Covering OWASP Top 10 across 34 languages
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

## Documentation

Full documentation at [vexlit.com/docs](https://vexlit.com/docs)

## License

MIT
