# Upload Configuration Action

*Requires that the workspace already exists within Terraform Cloud.*

Creates and uploads configuration files for a provided Terraform Cloud workspace.

This action will wait until the upload has been successful or has failed/timeout.

## Behaviors / Expected Outcomes
* A successful upload will return a `Success` and a status code of `0`.
* If the upload status results in a failured state, the action will return `Error` status and status code of `1`.
* If the upload status takes longer than the provided `TF_MAX_TIMEOUT` or default, will return `Timeout` and status code of `1`.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
jobs:
  terraform-plan-run:
    env:
      TF_CLOUD_ORGANIZATION: ${{ secrets.TF_CLOUD_ORGANIZATION }}
      TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
      TF_WORKSPACE: "my-workspace"
      ## Relative to the root of the project
      CONFIG_DIRECTORY: "./terraform"

    steps:
      - uses: actions/checkout@v3

      - uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.0.4
        id: upload
        with:
          workspace: ${{ env.TF_WORKSPACE }}
          directory: ${{ env.CONFIG_DIRECTORY }}
```
