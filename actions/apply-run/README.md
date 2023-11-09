# Apply Run Action

Applies a run that is paused waiting for confirmation after a plan. Requires a valid Terraform Cloud Run ID.

This action will wait until the apply has been successful or has failed/timeout.

## Behaviors / Expected Outcomes
* If there are no changes to apply, the action will return a `Noop` status and status code of `0`.
* If Apply reaches a failured state, will return `Error` status and status code of `1`.
* If Apply times out once `TF_MAX_TIMEOUT` has been reached, will return `Timeout` and status code of `1`. Does not mean the Apply will fail, however the action will stop waiting for the run to complete.
* A successful apply will return a `Success` and status code of `0`.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
- uses: hashicorp/tfc-workflows-github/actions/apply-run@v1.1.1
  # assign id attribute to reference in subsequent steps
  id: apply
  # if want to and handle automation if apply fails
  continue-on-error: true
  with:
    hostname: "my.tfe.instance.io" # if using Terraform Cloud Enterprise
    token: ${{ secrets.TF_API_TOKEN }} # recommend to store as repository secret
    run: ${{ steps.create-run.outputs.run_id }}
    comment: "Confirmed from GitHub Actions CI"
```


```yml
env:
  # No need to pass as inputs to each action, can add to environment
  TF_CLOUD_ORGANIZATION: ${{ secrets.TF_CLOUD_ORGANIZATION }}
  TF_API_TOKEN: ${{ secrets.TF_API_TOKEN }}

jobs:
  terraform-apply:
    runs-on: "ubuntu-latest"
    steps:
      ...

      - uses: hashicorp/tfc-workflows-github/actions/apply-run@v1.1.1
        # assign id attribute to reference in subsequent steps
        id: apply
        with:
          run: ${{ steps.create-run.outputs.run_id }}
          comment: "Apply Run from GitHub Actions CI ${{ github.sha }}"
```
