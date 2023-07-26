# Terraform Cloud Workflows for GitHub

This repo includes prescriptive workflows that implement best practices when interacting with Terraform Cloud. These starter workflow templates provide a entrypoint to integrate your CI/CD pipelines with Terraform Cloud.

## Related Projects
* [tfc-workflows-tooling](https://github.com/hashicorp/tfc-workflows-tooling)

## About

These templates use individual custom Docker GitHub Actions that interact with Terraform Cloud API's rather than the traditional Terraform CLI.

The core tooling is a containerized go application, designed to work with GitHub Actions, GitLab Pipelines, and more in the future.

## Starter Workflow Templates

* [Terraform Speculative Run (Pull Request Open/Edit Workflow)](https://github.com/hashicorp/tfc-workflows-github/blob/main/workflow-templates/terraform-cloud.speculative-run.workflow.yml): Workflow to run non-applyable speculative runs in Terraform Cloud when a pull request has been opened/modified.
* [Terraform Apply Run (Push to main branch/PR Merge to main Workflow)](https://github.com/hashicorp/tfc-workflows-github/blob/main/workflow-templates/terraform-cloud.apply-run.workflow.yml): Workflow to perform an apply run for a given workspace.


## Getting Started

See [`workflow/README.md`](https://github.com/hashicorp/tfc-workflows-github/blob/main/workflow-templates/README.md)

### GitHub Actions

See [`actions/README.md`](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/README.md)

Supported Actions:
* [upload-configuration](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/upload-configuration/action.yml): Creates and uploads configuration files for a given Terraform Cloud workspace.
* [create-run](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/create-run/action.yml): Performs a new plan run in Terraform Cloud, using a configuration version and the workspace’s current variables.
* [apply-run](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/apply-run/action.yml): Applies a run that is paused waiting for confirmation after a plan.
* [discard-run](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/discard-run/action.yml): Skips any remaining work on runs that are paused waiting for confirmation or priority.
* [cancel-run](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/cancel-run/action.yml): Interrupts a run that is currently planning or applying.
* [show-run](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/show-run/action.yml): Returns run details for the corresponding Run ID.
* [plan-output](https://github.com/hashicorp/tfc-workflows-github/blob/main/actions/plan-output/action.yml): Returns the plan details for the provided Plan ID.

## Contributing Guideline

See [`docs/CONTRIBUTING.md`](https://github.com/hashicorp/tfc-workflows-github/blob/main/docs/CONTRIBUTING.md)

## License

[Mozilla Public License v2.0](https://github.com/hashicorp/tfc-workflows-github/blob/main/LICENSE)
