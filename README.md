# Reusable Workflow Invoke Lint Lint

[![Dependabot](https://flat.badgen.net/github/dependabot/yukihiko-shinoda/reusable-workflow-invoke-lint-lint?icon=dependabot)](https://github.com/yukihiko-shinoda/reusable-workflow-invoke-lint-lint/security/dependabot)

The GitHub Actions reusable workflow for linting Python packages powered by [invoke-lint].

## Advantage

1. Eliminates duplicated CI linting configuration
2. Three parallel jobs for faster feedback

### 1. Eliminates duplicated CI linting configuration

Without a reusable workflow, each repository must define the same three-job linting setup individually.
A single `uses:` reference replaces that duplication.

### 2. Three parallel jobs for faster feedback

Style check, lint, and deep lint run concurrently as independent jobs,
so the total CI time equals the slowest job rather than their sum.

## Quickstart

Add a workflow file to the caller repository:

```yaml
# .github/workflows/lint.yml
name: Lint
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
permissions:
  contents: read
jobs:
  lint:
    uses: yukihiko-shinoda/reusable-workflow-invoke-lint-lint/.github/workflows/workflow.yml@v1
```

<!-- markdownlint-disable-next-line no-trailing-punctuation -->
## How do I...

### How do I enable Xenon for cyclomatic complexity checking?

Set `enable-xenon: true` to add the `--xenon` flag to the lint job:

```yaml
jobs:
  lint:
    uses: yukihiko-shinoda/reusable-workflow-invoke-lint-lint/.github/workflows/workflow.yml@v1
    with:
      enable-xenon: true
```

### How do I pin a specific Python version?

Pass the `python-version` input:

```yaml
jobs:
  lint:
    uses: yukihiko-shinoda/reusable-workflow-invoke-lint-lint/.github/workflows/workflow.yml@v1
    with:
      python-version: '3.12'
```

## API

### Inputs

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `python-version` | string | `'3.14'` | Python version passed to `astral-sh/setup-uv` |
| `enable-xenon` | boolean | `false` | Adds `--xenon` flag to the `check_lint` job for cyclomatic complexity enforcement |

[invoke-lint]: https://github.com/yukihiko-shinoda/invoke-lint
