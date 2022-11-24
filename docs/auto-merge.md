# auto-merge

The `auto-merge` reusable action is located in `.github/workflows/auto-merge.yml`.

> NOTE: This only auto merges pull requests opened by [Dependabot](https://github.com/dependabot).

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [dependabot-auto-merge](https://github.com/marketplace/actions/dependabot-auto-merge)

## Inputs

The action has the following inputs:

### Required inputs

#### target-repo

Specify the target repository this action should run on. This is used to prevent actions from running on repositories other than the target repository. For example, specifying a target-repo of `schalkneethling/schalkneethling.com` will prevent the workflow from running on forks of `schalkneethling/schalkneethling.com`.

- This input is required

### Secrets

This action requires the following secret to be passed by the caller. It needs to be set as [environmental secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) using the calling repositories settings.

#### GH_TOKEN

You will need a [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) with the following scopes:

- `repo` for private repositories
- `public_repo` for public repositories

### Usage

In the repository that will call this action, you will need to define a `.github/workflows/auto-merge.yml` file with the following content:

```yml
name: auto-merge
on: [pull_request_target]

jobs:
  auto-merge:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/auto-merge.yml@main
    with:
      target-repo: "schalkneethling/schalkneethling.com"
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
```
