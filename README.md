[![GitHub license](https://img.shields.io/github/license/op5dev/require-team-approval?logo=apache&label=License)](LICENSE "Apache License 2.0.")
[![GitHub release tag](https://img.shields.io/github/v/release/op5dev/require-team-approval?logo=semanticrelease&label=Release)](https://github.com/op5dev/require-team-approval/releases "View all releases.")
*
[![GitHub repository stargazers](https://img.shields.io/github/stars/op5dev/require-team-approval)](https://github.com/op5dev/require-team-approval "Become a stargazer.")

# Require Team Approval

Require N number of PR approvals from members of a GitHub team. Used to enforce pre-merge PR checks more explicitly than GitHub's native `CODEOWNERS` file.

### View: [Usage Examples](#usage-examples) | [Parameters](#parameters) | [Security](#security) | [Changelog](#changelog) | [License](#license)

</br>

## Usage Examples

### #1 `pull_request` event requiring 1 approval each from 2 teams

The bare minimum configuration requires a GitHub access token with 'read:org' scope and the team name whose approval is required. The default number of approvals is set to 1 on the PR which triggered the workflow and can be called multiple times for different teams.

```yaml
on:
  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest

steps:
  - name: Require DEV team approval
    uses: op5dev/require-team-approval@v1
    with:
      team: dev-team
      token: ${{ secrets.CI_PAT }}

  - name: Require QA team approval
    uses: op5dev/require-team-approval@v1
    with:
      team: qa-team
      token: ${{ secrets.CI_PAT }}
```

</br>

### #2 `pull_request_review` event requiring 2 approvals with outputs

This configuration demonstrates a more targeted approach by checking for approvals only when an `approved` review is submitted. The outputs provide detailed information about the approvals, both by the team and the PR in general, for use in subsequent steps as counts or named lists.

```yaml
on:
  pull_request_review:
    types: [submitted]

jobs:
  check:
    if: github.event.review.state == 'approved'
    runs-on: ubuntu-latest

    steps:
      - name: Require QA team approval x2
        id: approval
        uses: op5dev/require-team-approval@v1
        with:
          approvals: 2
          team: qa-team
          token: ${{ secrets.CI_PAT }}

      - run: |
          echo "Got ${{ steps.approval.outputs.team-approvals-count }} / 2 approvals from QA."
          echo "Got ${{ steps.approval.outputs.pr-approvals-count }} approvals in total."
```

</br>

## Parameters

### Inputs

| Name                   | Description                                                              |
| ---------------------- | ------------------------------------------------------------------------ |
| `team`</br>(required)  | Team whose approval is required.</br>Example: `qa-team`                  |
| `token`</br>(required) | GitHub access token with 'read:org' scope.</br>Example: `secrets.CI_PAT` |
| `approvals`            | Count of approvals required.</br>Default: `1`                            |
| `pr-number`            | Override PR number.</br>Example: `42`                                    |

</br>

### Outputs

| Name                   | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| `pr-approvals`         | List of approvals on the PR.</br>Example: `[dev-1,qa-1]` |
| `pr-approvals-count`   | Count of approvals on the PR.</br>Example: `1`           |
| `team-approvals`       | List of approvals from the team.</br>Example: `[qa-1]`   |
| `team-approvals-count` | Count of approvals from the team.</br>Example: `1`       |
| `team-members`         | List of team members.</br>Example: `[dev-1,dev-2,dev-3]` |
| `team-members-count`   | Count of team members.</br>Example: `1`                  |

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
- Copyright 2016-present [Rishav Dhar](https://github.com/rdhar "Rishav Dhar's GitHub profile.") — All wrongs reserved.
