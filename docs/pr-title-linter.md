# pr-title-linter

The `pr-title-linter` ensures that the title of a pull request following the [Conventional Commits specification](https://www.conventionalcommits.org/).

This reusable action depends on the following actions:

- [Semantic pull request](https://github.com/marketplace/actions/semantic-pull-request)
- [Sticky pull request comment](https://github.com/marketplace/actions/sticky-pull-request-comment)

## Inputs

The action has the following required inputs.

### Required inputs

#### target-repo

Specify the target repository this action should run on. This is used to prevent actions from running on repositories other than the target repository. For example, specifying a target-repo of `mechanical-ink/calavera-reusable-actions` will prevent the workflow from running on forks of `mechanical-ink/calavera-reusable-actions`.

- This input is required

### Optional inputs

The action has the following optional inputs.

#### required-scope

Whether scope is required. For example: `fix(ui)`, In this instance, `ui` is the scope.

- This input is optional with a default of `false`.

#### disallow-scopes

Configure which scopes (newline delimited) are disallowed in PR titles.

- This input is optional with a default of an empty string, i.e. allow all scopes.

#### show-error-message

Whether to add a comment to the pull request with the error message.

- This input is optional with a default of `true`.

#### pr-lint-error-message

Pull request comment to add to pull request if linting failed.

- This input is optional with the following default.

👋🏼 Thank you for opening a pull request and your interest in the project.

We require pull request titles to follow the [Conventional Commits specification](https://www.conventionalcommits.org/) and it looks like your proposed title needs to be adjusted.

Details:

```js
${{ steps.lint_pr_title.outputs.error_message }}
```

### Usage

In the repository that will call this action, you will need to add a `.github/workflows/pr-title-linter.yml` file with the following content.

#### With defaults

```yml
name: pr-title-linter

on:
  pull_request_target:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  pr-title-linter:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/pr-title-linter.yml@main
    with:
      target-repo: "mechanical-ink/calavera-reusable-actions"
```

#### Requiring scopes

```yml
name: pr-title-linter

on:
  pull_request_target:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  pr-title-linter:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/pr-title-linter.yml@main
    with:
      target-repo: "mechanical-ink/calavera-reusable-actions"
      required-scopes: false
```

#### Disallowing specific scopes

```yml
name: pr-title-linter

on:
  pull_request_target:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  pr-title-linter:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/pr-title-linter.yml@main
    with:
      target-repo: "mechanical-ink/calavera-reusable-actions"
      disallow-scopes: |
        release
        beta
```

#### Do not comment on failure

```yml
name: pr-title-linter

on:
  pull_request_target:
    branches:
      - main

jobs:
  pr-title-linter:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/pr-title-linter.yml@main
    with:
      target-repo: "mechanical-ink/calavera-reusable-actions"
      show-error-message: false
```

#### Using a custom failure comment

```yml
name: pr-title-linter

on:
  pull_request_target:
    branches:
      - main

permissions:
  contents: read
  pull-requests: write

jobs:
  pr-title-linter:
    uses: project-calavera/calavera-reusable-actions/.github/workflows/pr-title-linter.yml@main
    with:
      target-repo: "mechanical-ink/calavera-reusable-actions"
      pr-lint-error-message: |
        Your pull request title contains the following error(s):

        ```
        ${{ steps.lint_pr_title.outputs.error_message }}
        ```
```
