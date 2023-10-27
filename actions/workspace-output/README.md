# Workspace Output Action

Fetches the latest state version outputs for the provider Terraform Cloud workspace.

> [!Note]
Outputs marked as `sensitive` are redacted

## Action Inputs / Outputs

To view all available inputs and outputs, see `./action.yml` [file](./action.yml).

### workspace (required)

The name of the Terraform Cloud Workspace

## Example Usage

```yml
jobs:
  terraform-fetch-outputs:
    env:
      TF_CLOUD_ORGANIZATION: ${{ secrets.TF_CLOUD_ORGANIZATION }}
      TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}
      TF_WORKSPACE: "my-workspace"

    steps:
      - uses: actions/checkout@v3

      - uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.1.0
        id: fetch-outputs
        with:
          workspace: ${{ env.TF_WORKSPACE }}

      # If looking for output value named, "image_id"
      # can leverage looks like command like tools such as jq
      - name: display-outputs
      - run: |
          outval=$(echo '${{steps.fetch-outputs.outputs}}' | jq '.outputs[] | select(.name == "image_id").value' )
          echo $outval
```
