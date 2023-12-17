# reusable-workflows
A collection of reusable GitHub Actions workflows I use in my public and private projects.


## php-cs-fixer

Workflow to run `php-cs-fixer` and automatically fix code style violations. Changes are pushed back to the GitHub repository.

```yml
# .github/workflows/php-cs-fixer.yml
name: php-cs-fixer

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  php-cs-fixer:
    uses: stefanzweifel/reusable-workflows/.github/workflows/php-cs-fixer.yml@main
```

## `release-drafter.yml`

Workflow to run [release-drafter](https://github.com/release-drafter/release-drafter) in projects.
To prevent usage spikes, the job is not executed if the user is Dependabot.

```yml
# .github/workflows/release-drafter.yml
name: Release Drafter

on:
  push:
    branches:
      - main

jobs:
  update_release_draft:
    uses: stefanzweifel/reusable-workflows/.github/workflows/release-drafter.yml@main
```

## `update-changelog.yml`

Workflow to automatically update a projects `CHANGELOG.md` with the release notes of a new GitHub release.
The release name is used as a heading in the changelog. The release body is treated as release notes and will be placed below the release heading.

```yml
# .github/workflows/update-changelog.yml
name: "Update Changelog"

on:
  release:
    types: [released]

jobs:
  update
    uses: stefanzweifel/reusable-workflows/.github/workflows/update-changelog.yml@main
```
