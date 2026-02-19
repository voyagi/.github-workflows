# .github-workflows

Reusable GitHub Actions workflows for all voyagi repos.

## Quick Start

Create `.github/workflows/ci.yml` in your repo with one of the examples below.

## Available Workflows

### node-ci.yml

Node.js CI pipeline. Biome check on by default, everything else opt-in.

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/node-ci.yml@main
    with:
      run-build: true
      run-tests: true
```

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `node-version` | string | `"22"` | Node.js version |
| `run-biome` | boolean | `true` | Run Biome lint and format check |
| `run-build` | boolean | `false` | Run `npm run build` |
| `run-tests` | boolean | `false` | Run `npm test` |
| `run-trivy` | boolean | `false` | Run Trivy security scan |

Requires: `package-lock.json`, Biome as dev dependency (unless `run-biome: false`).

### python-ci.yml

Python CI pipeline. Ruff check on by default, everything else opt-in.

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/python-ci.yml@main
    with:
      dev-requirements-file: "requirements-dev.txt"
      run-tests: true
      run-mypy: true
```

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `"3.12"` | Python version |
| `requirements-file` | string | `"requirements.txt"` | Main requirements file |
| `dev-requirements-file` | string | `""` | Dev requirements file (empty to skip) |
| `run-tests` | boolean | `false` | Run `pytest` |
| `run-mypy` | boolean | `false` | Run `mypy .` |
| `run-trivy` | boolean | `false` | Run Trivy security scan |

Requires: `requirements.txt`. Ruff is installed automatically.

### deno-ci.yml

Deno CI pipeline. Lint and format check on by default.

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/deno-ci.yml@main
    with:
      run-tests: true
```

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `deno-version` | string | `"v2.x"` | Deno version |
| `run-tests` | boolean | `false` | Run `deno test` |

### dependency-review.yml

Scans PRs for newly-introduced vulnerable dependencies. Works for npm and pip.

```yaml
name: Dependency Review
on: [pull_request]
jobs:
  review:
    uses: voyagi/.github-workflows/.github/workflows/dependency-review.yml@main
```

### stale.yml

Marks and closes inactive issues. Does not touch PRs.

```yaml
name: Stale
on:
  schedule:
    - cron: "0 0 * * 0"
jobs:
  stale:
    uses: voyagi/.github-workflows/.github/workflows/stale.yml@main
```

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `days-before-stale` | number | `60` | Days before marking stale |
| `days-before-close` | number | `7` | Days after stale before closing |

### release.yml

Creates a GitHub Release with auto-generated notes when a version tag is pushed.

```yaml
name: Release
on:
  push:
    tags: ["v*"]
permissions:
  contents: write
jobs:
  release:
    uses: voyagi/.github-workflows/.github/workflows/release.yml@main
```
