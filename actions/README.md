# Terraform Cloud Workflow Actions

All of these actions assume Terraform Cloud resources such as Organization, Workspace, etc. already exist.

They do not create these resources for you. If you are looking for this type of functionality, look to the [`Terraform Cloud/Enterprise Provider`](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs).

## Shared Inputs

All of the provided actions include inputs for: `hostname`, `token`, `organization`.

For convenience, you are also able to specify these values within the GitHub Action runner as environment variables. If these values are set as environment variables, you can omit them as input to each individual action.

### `hostname`

**Optional** The hostname of a Terraform Enterprise installation, if using Terraform Enterprise. Defaults to Terraform Cloud (`app.terraform.io`) if `TF_HOSTNAME` environment variable is not set.

### `token`

**Optional** The token used to authenticate with Terraform Cloud. Defaults to reading `TF_API_TOKEN` environment variable. [Terraform Cloud API Token Docs](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens).

### `organization`

**Optional** The name of the organization in Terraform Cloud. Defaults to reading `TF_ORGANIZATION` environment variable.

## Environment Variables

| Variable Name     | Default            |  Description                                                                                                     |
| ----------------- |--------------------| ---------------------------------------------------------------------------------------------------------------- |
| `TF_HOSTNAME`     | `app.terraform.io` | The hostname of a Terraform Enterprise installation, if using Terraform Enterprise. Defaults to Terraform Cloud. |
| `TF_API_TOKEN`    | `n/a`              | The token used to authenticate with Terraform Cloud. [API Token Docs](https://developer.hashicorp.com/terraform/cloud-docs/users-teams-organizations/api-tokens)                                                           |
| `TF_ORGANIZATION` | `n/a`              | The name of the organization in Terraform Cloud.                                                                 |
| `TF_MAX_TIMEOUT`  | `1h`               | Max wait timeout to wait for actions to reach desired or errored state. ex: `1h30`, `30m`                                         |
| `TF_VAR_*`        | `n/a`              | Only applicable for create-run action. Note: strings must be escaped. ex: `TF_VAR_image_id="\"ami-abc123\""`. All values must be expressed as an HCL literal in the same syntax you would use when writing Terraform code. [Create Run API Docs](https://developer.hashicorp.com/terraform/cloud-docs/api-docs/run#create-a-run)                                 |
| `TF_LOG`          | `OFF`              | Debugging log level options: `OFF`, `ERROR`, `INFO`, `DEBUG`                                                     |


## Example Usage


```yml
- uses: hashicorp/tfc-workflows-github/actions/apply-run@v1.1.1
  # assign id attribute to reference in subsequent steps
  id: apply
  # if want to and handle automation if apply fails
  continue-on-error: true
  with:
    run: ${{ steps.create-run.outputs.run_id }}
    comment: "Confirmed from GitHub Actions CI"
    ## Can specify hostname,token,organization as direct inputs
    hostname: "my.tfe.instance.io" # if using Terraform Cloud Enterprise
    organization: ${{ vars.TF_CLOUD_ORGANIZATION }} # Configured as GitHub configuration variable
    token: ${{ secrets.TF_API_TOKEN }} # Configured as GitHub Secret
    ## OR can specify as environment variables per step, job, or entire workflow file.
    # env:
      # TF_CLOUD_ORGANIZATION: ${{ vars.TF_CLOUD_ORGANIZATION }} # Configured as GitHub configuration variable
      # TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }} # Configured as GitHub Secret
```

#### Setting Environment Variables for entire workflow

```yml
name: CI
on:
  push:
    branches:
      - main
    paths:
      # Replace with your directory, relative to the root of the project
      - 'terraform/**.tf'

env:
  # No need to pass as inputs to each action
  TF_CLOUD_ORGANIZATION: ${{ secrets.TF_CLOUD_ORGANIZATION }}
  TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
  TF_WORKSPACE: "my-workspace"
  TF_DIRECTORY: "./terraform"

jobs:
  terraform-apply:
    runs-on: "ubuntu-latest"
    steps:
      - uses: actions/checkout@v3
      - uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.1.1
        id: upload
        with:
          workspace: ${{ env.TF_WORKSPACE }}
          directory: ${{ env.TF_DIRECTORY }}

      - uses: hashicorp/tfc-workflows-github/actions/create-run@v1.1.1
        id: create-run
        with:
          workspace: ${{ env.TF_WORKSPACE }}
          configuration_version: ${{ steps.upload.outputs.configuration_version_id }}

      - uses: hashicorp/tfc-workflows-github/actions/apply-run@v1.1.1
        # assign id attribute to reference in subsequent steps
        id: apply
        with:
          run: ${{ steps.create-run.outputs.run_id }}
          comment: "Confirmed from GitHub Actions CI"
```
