# set-default-labels

The `set-default-labels` reusable action is located in `.github/workflows/set-default-labels.yml`.

This reusable action depends on the following actions:

- [checkout](https://github.com/marketplace/actions/checkout)
- [lannonbr/issue-label-manager-action](https://github.com/marketplace/actions/issue-label-manager-action)

## Inputs

The action has the following inputs.

### Required inputs

#### target-repo

Specify the target repository this action should run on. This is used to prevent actions from running on repositories other than the target repository. For example, specifying a target-repo of `schalkneethling/schalkneethling.com` will prevent the workflow from running on forks of `schalkneethling/schalkneethling.com`.

- This input is required

### should-delete-labels

This is an optional `boolean` input that is `false` by default. If set to `true`, the action will delete any existing labels that are not listed in the JSON file mentioned previously.

## Usage

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
    with:
      target-repo: "schalkneethling/schalkneethling.com"
```

Setting `should-delete-labels` to `true`.

```yml
name: set-default-labels
on: [workflow_dispatch]

jobs:
  set-default-labels:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/set-default-labels.yml@main
    with:
      target-repo: "schalkneethling/schalkneethling.com"
      should-delete-labels: true
```

Because of the nature of this action, it must be run manually. You can learn more about [manually running actions on GitHub](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow).
