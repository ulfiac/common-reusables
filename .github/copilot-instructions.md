# Copilot Instructions for github-action-reusables

## Repository Overview
This repository, `github-action-reusables`, serves as a centralized hub for reusable GitHub Actions workflows. It provides standardized, maintainable CI/CD components that can be shared across multiple repositories within the ulfiac organization.

## Key Components
- **Reusable Workflows**: Located in `.github/workflows/`, these are designed to be called from other repositories using `workflow_call`.
- **Tool Version Management**: `mise.toml` at the root specifies tool versions (e.g., Terragrunt) for consistent environment setup.
- **Primary Workflow**: `reusable_linter.yaml` - A comprehensive linting workflow that performs multiple checks:
  - GitHub Actions workflow linting (actionlint)
  - Shell script linting (shellcheck)
  - Terraform formatting validation (terraform fmt)
  - Terraform static analysis (tflint)
  - HCL formatting validation (terragrunt hcl fmt)
  - Security scanning of configuration files (trivy config)

## Interrelations with Other Repositories
This repository is used by several other repositories in the workspace as a shared dependency:

- **aws-bootstrap**: Uses the reusable linter workflow to validate Terraform configurations and scripts during CI/CD.
- **aws-infrastructure**: Leverages the linting workflow for its Terraform code and shell scripts.
- **github-repos**: Utilizes the workflow for maintaining code standards in its Terraform configurations.
- **vscode-workspaces**: While primarily a workspace configuration, it may benefit from the shared workflows if expanded.

## Usage Pattern
Other repositories reference this workflow using:
```yaml
uses: ulfiac/github-action-reusables/.github/workflows/reusable_linter.yaml@main
```

## Development Guidelines
- Keep workflows generic and configurable through inputs.
- Use `mise.toml` for specifying a tool's version only if the tool requires it.
- Test changes locally and in dependent repositories before merging.
- Update this instruction file when adding new reusable workflows or significant changes.

## Security Considerations
- Workflows run with minimal permissions (contents: read).
- Use pinned action versions for reproducibility and security.
- Regularly update dependencies and review security advisories.
