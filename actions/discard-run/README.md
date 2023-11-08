# Discard Run Action

Skips any remaining work on runs that are paused waiting for confirmation or priority.

Requires a valid Terraform Cloud Run ID.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
- uses: hashicorp/tfc-workflows-github/actions/discard-run@v1.1.1
  id: discard
  with:
    hostname: "my.tfe.instance.io" # if using Terraform Cloud Enterprise
    token: ${{ secrets.TF_API_TOKEN }} # recommend to store as repository secret
    run: ${{ steps.create-run.outputs.run_id }}
    comment: "Discarded from GitHub Actions CI"
```
