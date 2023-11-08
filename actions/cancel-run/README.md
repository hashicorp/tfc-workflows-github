# Cancel Run Action

Interrupts a run that is currently planning or applying.

Requires a valid Terraform Cloud Run ID.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
- uses: hashicorp/tfc-workflows-github/actions/cancel-run@v1.1.1
  id: cancel
  with:
    hostname: "my.tfe.instance.io" # if using Terraform Cloud Enterprise
    token: ${{ secrets.TF_API_TOKEN }} # recommend to store as repository secret
    run: ${{ steps.show-run.outputs.run_id }}
    comment: "Cancelled from GitHub Actions CI"
```
