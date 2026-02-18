# .github-workflows

Reusable GitHub Actions workflows for all voyagi repos.

## Available Workflows

### node-ci.yml

Node.js CI pipeline with Biome linting. Build, test, and Trivy scan are opt-in.

**Always runs:** checkout, npm ci, Biome check

**Optional:** build, tests, Trivy dependency scan

## Usage

Create `.github/workflows/ci.yml` in your repo:

### Minimal (Biome only)

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/node-ci.yml@main
```

### All checks enabled

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/node-ci.yml@main
    with:
      run-build: true
      run-tests: true
      run-trivy: true
```

### Custom Node.js version

```yaml
name: CI
on: [push, pull_request]
jobs:
  ci:
    uses: voyagi/.github-workflows/.github/workflows/node-ci.yml@main
    with:
      node-version: "20"
      run-tests: true
```

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `node-version` | string | `"22"` | Node.js version |
| `run-build` | boolean | `false` | Run `npm run build` |
| `run-tests` | boolean | `false` | Run `npm test` |
| `run-trivy` | boolean | `false` | Run Trivy security scan |

## Requirements

Consuming repos must have:

- `package-lock.json` (for `npm ci`)
- Biome as a dev dependency (`@biomejs/biome`)
- `biome.json` config file
