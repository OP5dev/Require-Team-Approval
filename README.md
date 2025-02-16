[![GitHub license](https://img.shields.io/github/license/op5dev/require-team-approval?logo=apache&label=License)](LICENSE "Apache License 2.0.")
[![GitHub release tag](https://img.shields.io/github/v/release/op5dev/require-team-approval?logo=semanticrelease&label=Release)](https://github.com/op5dev/require-team-approval/releases "View all releases.")
*
[![GitHub repository stargazers](https://img.shields.io/github/stars/op5dev/require-team-approval)](https://github.com/op5dev/require-team-approval "Become a stargazer.")

# Require Team Approval

Require N number of PR approvals from a GitHub team.

</br>

## Usage Examples

### #1 `pull_request` event with 1 approval

```yaml
on:
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest

steps:
  - uses: op5dev/require-team-approval@v1
    with:
      team: qa-team
      token: ${{ secrets.CI_PAT }}
```

### #2 `pull_request_review` event with 2 approvals and outputs

```yaml
on:
  pull_request_review:
    types: [submitted]

jobs:
  check:
    if: github.event.review.state == 'approved'
    runs-on: ubuntu-latest

    steps:
      - id: approval
        uses: op5dev/require-team-approval@v1
        with:
          approvals: 2
          team: qa-team
          token: ${{ secrets.CI_PAT }}

      - run: |
          echo "Needed 2 approvals from QA team, got ${{ steps.approval.outputs.team-approvals-count }}."
          echo "PR received ${{ steps.approval.outputs.team-approvals }} approvals in total."
```
