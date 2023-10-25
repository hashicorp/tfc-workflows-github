# Upload Configuration Action

*Requires that the workspace already exists within Terraform Cloud.*

Creates and uploads configuration version containing files for a specified Terraform Cloud Workspace.

This action will wait until the upload has been successful or has failed/timeout.

## Behaviors / Expected Outcomes
* A successful upload will return a `Success` and a status code of `0`.
* If the upload status results in a failured state, the action will return `Error` status and status code of `1`.
* If the upload status takes longer than the provided `TF_MAX_TIMEOUT` or default, will return `Timeout` and status code of `1`.

## Action Inputs / Outputs

To view all available inputs and outputs, see `./action.yml` [file](./action.yml).

### workspace (required)

The name of the Terraform Cloud Workspace

### directory (required)

Filesystem path containing the Terraform Configuration to upload.

You can specify the relative or absolute path to the Terraform configuration in your project.

GitHub Docker container actions maps the default working directory (`GITHUB_WORKSPACE`) on the action runner with the `/github/workspace` directory on the container.


#### Example
With the following **Project structure**

```
├── .github/
│   └── workflows/
│       └── terraform-cloud-speculative.workflows.yml
└── terraform/
    └── main.tf
```

##### Relative Path Example

***terraform-cloud-speculative.workflows.yml***
```yml
# ...
- uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.0.4
  id: upload
  with:
    workspace: ${{ env.TF_WORKSPACE }}
    directory: "./terraform"
```

##### Absolute Path Example

***terraform-cloud-speculative.workflows.yml***
```yml
# ...
- uses: hashicorp/tfc-workflows-github/actions/upload-configuration@v1.0.4
  id: upload
  with:
    workspace: ${{ env.TF_WORKSPACE }}
    directory: "/github/workspace/terraform"
```

### speculative

When true, this configuration version may only be used to create runs which are speculative, that is, can neither be confirmed nor applied.

Default: `false`

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
