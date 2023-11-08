# Plan Output Action

Returns the plan details for the provided Plan ID.

## Action Inputs / Outputs

See `./action.yml` file for all available inputs and outputs.

## Example Usage

```yml
- uses: hashicorp/tfc-workflows-github/actions/plan-output@v1.1.1
  id: plan-output
  with:
    plan: ${{ steps.run.outputs.plan_id }}

- name: Reference Plan Output
  run: |
    echo "Plan status: ${{ steps.plan-output.outputs.plan_status }}"
    echo "Resources to Add: ${{ steps.plan-output.outputs.add }}"
    echo "Resources to Change: ${{ steps.plan-output.outputs.change }}"
    echo "Resources to Destroy: ${{ steps.plan-output.outputs.destroy }}"

```
