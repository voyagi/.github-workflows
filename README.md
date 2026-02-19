# .github-workflows

Reusable GitHub Actions workflows for all voyagi repos.

## Available Workflows

### node-ci.yml

Node.js CI pipeline with Biome linting. Build, test, and Trivy scan are opt-in.

**Always runs:** checkout, npm ci, Biome check

**Optional:** build, tests, Trivy dependency scan

#### Usage

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

#### Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `node-version` | string | `"22"` | Node.js version |
| `run-build` | boolean | `false` | Run `npm run build` |
| `run-tests` | boolean | `false` | Run `npm test` |
| `run-trivy` | boolean | `false` | Run Trivy security scan |

#### Requirements

- `package-lock.json` (for `npm ci`)
- Biome as a dev dependency (`@biomejs/biome`)
- `biome.json` config file

### python-ci.yml

Python CI pipeline with Ruff linting. Tests, mypy, and Trivy scan are opt-in.

**Always runs:** checkout, pip install, Ruff check, Ruff format check

**Optional:** pytest, mypy, Trivy dependency scan

#### Usage

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

#### Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `python-version` | string | `"3.12"` | Python version |
| `requirements-file` | string | `"requirements.txt"` | Main requirements file |
| `dev-requirements-file` | string | `""` | Dev requirements file (empty to skip) |
| `run-tests` | boolean | `false` | Run `pytest` |
| `run-mypy` | boolean | `false` | Run `mypy .` |
| `run-trivy` | boolean | `false` | Run Trivy security scan |

#### Requirements

- `requirements.txt` (or custom file via input)
- Ruff is installed automatically (works with zero config)
