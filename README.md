# github-action-reusables

This repository serves as a centralized hub for reusable GitHub Actions workflows within the ulfiac organization. It provides standardized, maintainable CI/CD components that ensure consistency across multiple repositories.

## Purpose

The primary goal is to reduce duplication and maintain high code quality standards by offering shared workflows that can be easily integrated into any repository. This approach simplifies maintenance, ensures security best practices, and allows for centralized updates to CI/CD processes.

## Tool Version Management

Tool versions are managed using `mise.toml` at the repository root to ensure consistent environments across different systems and CI runs.

## Reusable Workflows

### reusable_linter.yaml

A comprehensive linting workflow that performs multiple code quality checks. This workflow can be called from other repositories to validate code before merging.

#### Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `actionlint` | boolean | `true` | Enable GitHub Actions workflow linting |
| `shellcheck` | boolean | `true` | Enable shell script linting |
| `terraform-fmt` | boolean | `true` | Enable Terraform formatting validation |
| `terragrunt-hcl-fmt` | boolean | `true` | Enable HCL formatting validation using Terragrunt |
| `tflint` | boolean | `true` | Enable Terraform static analysis |
| `trivy-config` | boolean | `true` | Enable security scanning of configuration files |

#### Usage Example

To use this workflow in another repository, add the following to your `.github/workflows/linter.yaml`:

```yaml
name: linter
run-name: "linter: ${{ github.event_name == 'pull_request' && github.event.pull_request.title || github.ref_name }}"

on:
  pull_request:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read

jobs:
  reusable-linter:
    uses: ulfiac/github-action-reusables/.github/workflows/reusable_linter.yaml@main
    with:
      shellcheck: false
```

The above example disables the shellcheck linter; the other linters remain enabled.  All linters are enabled by default but can be selectively disabled if needed.

## Contributing

When adding new reusable workflows:
- Ensure workflows are generic and configurable through inputs
- Use pinned action versions for security and reproducibility
- Update tool versions in `mise.toml` as needed
- Test changes in dependent repositories before merging
- Update this README with new workflow documentation
- Update the copilot instructions

## Security

- Workflows run with minimal required permissions
- All actions use pinned versions to prevent supply chain attacks
- Regular security audits and dependency updates are performed
