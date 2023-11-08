# Show Run Action

Returns run details for the provided Terraform Cloud run ID.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
  - ...
  - uses: hashicorp/tfc-workflows-github/actions/show-run@v1.1.1
    id: run
    with:
      run: ${{ steps.reference-run-id.outputs.run_id }}

  - run: |
      echo "Latest Run Status: ${{ steps.run.outputs.run_status }}"
```
