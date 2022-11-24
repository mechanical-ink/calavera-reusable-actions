# publish-vscode-extension

The `publish-vscode-extension` reusable action is located in `.github/workflows/publish-vscode-extension.yml`. To use it you will need a [Personal Access Token](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#get-a-personal-access-token) named `VSCODE_MARKETPLACE_TOKEN`.

In the repository that will call this action, you need to [define a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) named `VSCODE_MARKETPLACE_TOKEN` with the value of your Personal Access Token.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [lannonbr/vsce-action](https://github.com/marketplace/actions/github-action-for-vsce)

## Usage

In the repository that will call this action, you will need to define a `.github/workflows/publish-vscode-extension.yml` file with the following content:

### Required inputs

#### target-repo

Specify the target repository this action should run on. This is used to prevent actions from running on repositories other than the target repository. For example, specifying a target-repo of `schalkneethling/schalkneethling.com` will prevent the workflow from running on forks of `schalkneethling/schalkneethling.com`.

- This input is required

```yml
name: publish-vscode-extension
on:
  push:
    branches:
      - main

jobs:
  publish-vscode-extension:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/publish-vscode-extension.yml@main
    with:
      target-repo: "schalkneethling/schalkneethling.com"
    secrets:
      VSCODE_MARKETPLACE_TOKEN: ${{ secrets.VSCODE_MARKETPLACE_TOKEN }}
```
