# Create Run Action

*Requires that the workspace already exists within Terraform Cloud.*

Performs an API driven run in Terraform Cloud, using a configuration version and the workspace's current variables. If a configuration version is not provided, will default to the workspace's most recently used version. [Terraform Cloud Create-Run API](https://developer.hashicorp.com/terraform/cloud-docs/api-docs/run#create-a-run).

This action will wait until the run status has reached a terminal/final state.

[Terraform Cloud API-driven Run Workflow](https://developer.hashicorp.com/terraform/cloud-docs/run/api)

## Behaviors / Expected Outcomes
* If the Plan reaches a failured state, will return `Error` status and status code of `1`.
* If the Workspace is locked, will return a `Error` status and status code of `1`.
* If the Plan timesout once `TF_MAX_TIMEOUT` has been reached, will return a `Timeout` and status code of `1`. Does not mean the Plan will fail, however the action will stop waiting for the Plan to complete.
* A successful Plan will return a `Success` and status code of `0`.

## Action Inputs / Outputs

To view all available inputs and outputs, see `./action.yml` [file](./action.yml).

### Variables

#### `TF_VAR_*` Environment variable(s)
Terraform Cloud API runs allow you to specify a list of variables using key and value attributes. These variables can be configured using environment variables with the `TF_VAR_*` naming convention. These variables take precedence over variables with the same key applied to the workspace(e.g., variable sets). For greater flexibility and features, it's recommended to use Terraform Cloud [workspace variables and variable sets](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/variables).

> [!Note]
All values must be expressed as an HCL literal in the same syntax you would use when writing Terraform code. Variables expressed as `string` will need to be escaped to include double quotations like, `TF_VAR_image_id="\"ami-my-custom-image\""` or `TF_VAR_image_id='"ami-my-custom-image'"`. [Read more on Terraform complex typed values](https://developer.hashicorp.com/terraform/language/values/variables#complex-typed-values)


In your GitHub Action workflow file, you can specify variables using the `env` keyword at either the job or workflow level.

```yml
env:
  TF_VAR_image_id="\"ami-abc123\"" ## string variable
  TF_VAR_user_map='{"1":{"name":"jason bourne","size":2}}'` # map of objects variable
  TF_VAR_availability_zone_names='["us-east-1a","us-west-1c", "us-west-2b"]'` # list of strings variable
```

Your `*.tf` configuration
```terraform
variable "image_id" {
  type = string
}

variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}


variable "user_map" {
  type = map(object({
    name = string
    size = number
  }))
}
```

All values must be expressed as an HCL literal in the same syntax you would use when writing Terraform code

## Example Usage

Like with the other actions, you can specify hostname, token, and organization as inputs OR as environment variables.

This specific example does not pass a configuraiton version id, so the run defaults to the workspace's most recently used version. [Read more about create run behavior in the Terraform Cloud API](https://developer.hashicorp.com/terraform/cloud-docs/api-docs/run#create-a-run)

```yml
- uses: hashicorp/tfc-workflows-github/actions/create-run@v1.1.1
  id: run
  with:
    workspace: "my-workspace"
    plan_only: true
    message: "Plan Run from GitHub Actions"
    ## Can specify hostname,token,organization as direct inputs
    hostname: ""
    organization: ${{ vars.TF_CLOUD_ORGANIZATION }} # Configured as GitHub configuration variable
    token: ${{ secrets.TF_API_TOKEN }} # Configured as GitHub Secret
    ## OR
    # env:
      # TF_CLOUD_ORGANIZATION: ${{ vars.TF_CLOUD_ORGANIZATION }} # Configured as GitHub configuration variable
      # TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }} # Configured as GitHub Secret
```

Creating a new run with recently uploaded configuration in a previous step

```yml
- uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.1.1
  id: upload
  with:
    workspace: ${{ env.TF_WORKSPACE }}
    directory: ${{ env.TF_DIRECTORY }}

- uses: hashicorp/tfc-workflows-github/actions/create-run@v1.1.1
  id: run
  with:
    workspace: ${{ env.TF_WORKSPACE }}
    configuration_version: ${{ steps.upload.outputs.configuration_version_id }}
```
