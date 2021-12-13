# calavera-reusable-actions

A selection of reusable GitHub Actions

## auto-merge

The `auto-merge` reusable action is located in `.github/workflows/auto-merge.yml`. To use it you will need a [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) with the following scopes:

- `repo` for private repositories
- `public_repo` for public repositories

In the repository that will call this action, you need to [define a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) named `GH_TOKEN` with the value of your Personal Access Token.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [dependabot-auto-merge](https://github.com/marketplace/actions/dependabot-auto-merge)

### Usage

In the repository that will call this action, you will need to define a `.github/workflows/auto-merge.yml` file with the following content:

```yml
name: auto-merge
on: [pull_request_target]

jobs:
  auto-merge:
    uses:  project-calavera/calavera-reusable-actions/.github/workflows/auto-merge.yml@main
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

## publish-vscode-extension

The `publish-vscode-extension` reusable action is located in `.github/workflows/publish-vscode-extension.yml`. To use it you will need a [Personal Access Token](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#get-a-personal-access-token) named `VSCODE_MARKETPLACE_TOKEN`.

In the repository that will call this action, you need to [define a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) named `VSCODE_MARKETPLACE_TOKEN` with the value of your Personal Access Token.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [actions/setup-node](https://github.com/marketplace/actions/setup-node-js-environment)

It will also install:

- [vcse](https://www.npmjs.com/package/vsce)

### Usage

In the repository that will call this action, you will need to define a `.github/workflows/publish-vscode-extension.yml` file with the following content:

```yml
name: publish-vscode-extension
on:
  push:
    branches:
      - main

jobs:
  publish-vscode-extension:
    uses:  project-calavera/calavera-reusable-actions/.github/workflows/publish-vscode-extension.yml@main
    secrets:
      VSCODE_MARKETPLACE_TOKEN: ${{ secrets.VSCODE_MARKETPLACE_TOKEN }}
```

The default version of Node.js used is 14.x. If you need a difference version, you can specify it as follows:

```yml
name: publish-vscode-extension
on:
  push:
    branches:
      - main

jobs:
  publish-vscode-extension:
    uses:  project-calavera/calavera-reusable-actions/.github/workflows/publish-vscode-extension.yml@main
    with:
      node-version: 12
    secrets:
      VSCODE_MARKETPLACE_TOKEN: ${{ secrets.VSCODE_MARKETPLACE_TOKEN }}
```
