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
    uses: project-calavera/calavera-reusable-actions/.github/workflows/auto-merge.yml@main
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

## publish-vscode-extension

The `publish-vscode-extension` reusable action is located in `.github/workflows/publish-vscode-extension.yml`. To use it you will need a [Personal Access Token](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#get-a-personal-access-token) named `VSCODE_MARKETPLACE_TOKEN`.

In the repository that will call this action, you need to [define a secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) named `VSCODE_MARKETPLACE_TOKEN` with the value of your Personal Access Token.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [lannonbr/vsce-action](https://github.com/marketplace/actions/github-action-for-vsce)

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
    uses: project-calavera/calavera-reusable-actions/.github/workflows/publish-vscode-extension.yml@main
    secrets:
      VSCODE_MARKETPLACE_TOKEN: ${{ secrets.VSCODE_MARKETPLACE_TOKEN }}
```

## set-default-labels

The `set-default-labels` reusable action is located in `.github/workflows/set-default-labels.yml`.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [lannonbr/issue-label-manager-action](https://github.com/marketplace/actions/issue-label-manager-action)

### Usage

In the repository that will call this action, you will need to create the following file: `.github/labels.json`. The content of the file can be something like the following:

```json
[
  {
    "name": "bug",
    "color": "#d73a4a",
    "description": "something isn’t working"
  },
  {
    "name": "chore",
    "color": "#fef2c0",
    "description": "keeping the lights on"
  }
]
```

You can find more details about the above on the [issue-label-manager-action documentation](https://github.com/marketplace/actions/issue-label-manager-action#issue-label-manager-action). The next item you need to create in the repository that will call this action, is a workflow file. You can name the file `.github/workflows/set-default-labels.yml` and add the following content:

```yml
name: set-default-labels
on: [workflow_dispatch]

jobs:
  set-default-labels:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/set-default-labels.yml@main
```

### Inputs

The action has the following inputs:

#### should-delete-labels

This is an optional `boolean` input that is `false` by default. If set to `true`, the action will delete any existing labels that are not listed in the JSON file mentioned previously.

```yml
name: set-default-labels
on: [workflow_dispatch]

jobs:
  set-default-labels:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/set-default-labels.yml@main
    with:
      should-delete-labels: true
```

Because of the nature of this action, it must be run manually. You can learn more about [manually running actions on GitHub](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow).

## publish-release

The `publish-release` GitHub Action automates publication of a new release on GitHub, updates the changelog and also publishes to the NPM registry.

> NOTE: For the `release-please` workflow to work effectively, you need to follow the conventional commits conventions as [detailed here](https://www.conventionalcommits.org). You can also find additional documentation on the [GitHub Actions README](https://github.com/googleapis/release-please#how-should-i-write-my-commits).

This reusable action depends on the following actions:

- [GoogleCloudPlatform/release-please-action](https://github.com/googleapis/release-please)
- [checkout](https://github.com/marketplace/actions/checkout)
- [actions/setup-node](https://github.com/actions/setup-node)

## Inputs

The action has the following inputs:

### release-type

This is can be one of the release types as [detailed in the release please docs](https://github.com/googleapis/release-please#release-types-supported).

- This `input` is optional with a default of `node`

### node-version

The version of Node.js to use for the release. This action supports all [active and maintenance releases](https://nodejs.org/en/about/releases/) of Node.js.

- This `input` is optional with a default of `12`

### npm-publish

Whether to publish the package to the NPM registry.

- This `input` is optional with a default of `true`

### npm-publish-args

Arguments to pass to the `npm publish` command. This is ignored if `npm-publish` is set to `false`.

- This `input` is optional with a default of an empty string

### registry-url

The registry to publish to.

- This `input` is optional with a default of `https://registry.npmjs.org`

## Secrets

This action requires the following secrets to be passed by the caller. Both of these need to be set as [environmental secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) using the calling repositories settings.

### GH_TOKEN

Personal access token passed from the caller workflow. Read the documentation on [creating a PA token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token).

### NPM_AUTH_TOKEN

Authentication token for the NPM registry. Read the documentation for details on [creating a token](https://docs.npmjs.com/creating-and-viewing-access-tokens).

> NOTE: When skipping NPM publishing, this token is not required.

## Usage

In the repository that will call this action, you will need to add a `.github/workflows/publish-release.yml` file with the following content:

### With defaults

```yml
name: publish-release

on:
  push:
    branches:
      - main

jobs:
  publish-release:
    uses: mdn/workflows/.github/workflows/publish-release.yml@main
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
      NPM_AUTH_TOKEN: ${{ secrets.NPM_AUTH_TOKEN }}
```

### Overriding some defaults

```yml
name: publish-release

on:
  push:
    branches:
      - main

jobs:
  publish-release:
    uses: mdn/workflows/.github/workflows/publish-release.yml@main
    with:
      release-type: python
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
      NPM_AUTH_TOKEN: ${{ secrets.NPM_AUTH_TOKEN }}
```

### Skip NPM publishing

```yml
name: publish-release

on:
  push:
    branches:
      - main

jobs:
  publish-release:
    uses: mdn/workflows/.github/workflows/publish-release.yml@main
    with:
      npm-publish: false
    secrets:
      GH_TOKEN: ${{ secrets.GH_TOKEN }}
```
