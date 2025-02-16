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

</br>

## Parameters

### Inputs

| Name        | Description                                                         | Required |
| ----------- | ------------------------------------------------------------------- | -------- |
| `approvals` | Count of approvals required (e.g., 1).                              | `false`  |
| `pr-number` | Pull request number (e.g., 42).                                     | `false`  |
| `team`      | Team whose approval is required (e.g., qa-team).                    | `true`   |
| `token`     | GitHub access token with 'read:org' scope (e.g., `secrets.CI_PAT`). | `true`   |

</br>

### Outputs

| Name                   | Description                             |
| ---------------------- | --------------------------------------- |
| `pr-approvals`         | List of approvals on the pull request.  |
| `pr-approvals-count`   | Count of approvals on the pull request. |
| `team-approvals`       | List of approvals from the team.        |
| `team-approvals-count` | Count of approvals from the team.       |
| `team-members`         | List of team members.                   |
| `team-members-count`   | Count of team members.                  |

</br>

## Security

View [security policy and reporting instructions](SECURITY.md).

</br>

## Changelog

View [all notable changes](https://github.com/op5dev/require-team-approval/releases "Releases.") to this project in [Keep a Changelog](https://keepachangelog.com "Keep a Changelog.") format, which adheres to [Semantic Versioning](https://semver.org "Semantic Versioning.").

> [!TIP]
>
> All forms of **contribution are very welcome** and deeply appreciated for fostering open-source projects.
>
> - [Create a PR](https://github.com/op5dev/require-team-approval/pulls "Create a pull request.") to contribute changes you'd like to see.
> - [Raise an issue](https://github.com/op5dev/require-team-approval/issues "Raise an issue.") to propose changes or report unexpected behavior.
> - [Open a discussion](https://github.com/op5dev/require-team-approval/discussions "Open a discussion.") to discuss broader topics or questions.
> - [Become a stargazer](https://github.com/op5dev/require-team-approval/stargazers "Become a stargazer.") if you find this project useful.

</br>

## License

- This project is licensed under the permissive [Apache License 2.0](LICENSE "Apache License 2.0.").
- All works herein are my own, shared of my own volition, and [contributors](https://github.com/op5dev/require-team-approval/graphs/contributors "Contributors.").
- Copyright 2016-2025 [Rishav Dhar](https://github.com/rdhar "Rishav Dhar's GitHub profile.") — All wrongs reserved.
