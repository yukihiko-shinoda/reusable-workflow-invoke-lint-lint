# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository provides a reusable GitHub Actions workflow (`workflow_call`) that runs linting checks for Python projects using [invoke-lint](https://github.com/yukihiko-shinoda/invoke-lint). Callers include it via `uses: yukihiko-shinoda/reusable-workflow-invoke-lint-lint/.github/workflows/workflow.yml@v1`.

## Workflow architecture

The single file [.github/workflows/workflow.yml](.github/workflows/workflow.yml) defines three parallel jobs, all using `uv` for environment setup:

| Job | Command | Purpose |
| --- | --- | --- |
| `check_style` | `uv run invoke style --check` | Code formatting check |
| `check_lint` | `uv run invoke lint [--xenon]` | Linting (optionally with complexity gate) |
| `check_lint_deep` | `uv run invoke lint.deep` | Deep lint analysis |

## Inputs

| Input | Type | Default | Description |
| --- | --- | --- | --- |
| `python-version` | string | `'3.14'` | Python version passed to `astral-sh/setup-uv` |
| `enable-xenon` | boolean | `false` | Adds `--xenon` flag to the `lint` job for cyclomatic complexity enforcement |

## Dependency updates

Dependabot is configured to update GitHub Actions dependencies weekly ([.github/dependabot.yml](.github/dependabot.yml)). When action versions are bumped (e.g. `actions/checkout`, `astral-sh/setup-uv`), update all three jobs in `workflow.yml` consistently — each job pins its own version.
