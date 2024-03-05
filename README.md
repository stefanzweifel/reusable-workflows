# reusable-workflows
A collection of reusable GitHub Actions workflows I use in my public and private projects.


## auto-merge-dependabot-pr.yml

Workflow that can automatically merge Dependabot pull requests after the CI builds has run successfully. Below is an example workflow you can use in your repository. The workflow is executed when the "Integrate" ran completed successfully.

```yml
# .github/workflows/auto-merge.yml
name: Merge me!

on:
  workflow_run:
    types:
      - completed
    workflows:
      - 'Integrate'

jobs:
  merge-me:
    uses: stefanzweifel/reusable-workflows/.github/workflows/auto-merge-dependabot-pr.yml@main
    secrets:
      MERGE_ME_GITHUB_TOKEN: ${{ secrets.MERGE_ME_GITHUB_TOKEN }}
```

## `backup-restore.yml`

Workflow to run [laravel-backup-restore](https://github.com/stefanzweifel/laravel-backup-restore) and restore a database backup in a Laravel app. The backup must be stored on an AWS S3 bucket.

The passed `app_name` will be used as `APP_NAME` and will be used to find the latest backup for your app.

```yml
# .github/workflows/backup-restore.yml
name: backup

on:
  workflow_dispatch:
  schedule:
    - cron: "0 14 1 * *"

jobs:
  restore:
    uses: stefanzweifel/reusable-workflows/.github/workflows/backup-restore.yml@main
    with:
      # Value of `APP_NAME` env variable. Used to locate backup on remote disk.
      app_name: 'My Laravel App'
      php_version: '8.3'
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      AWS_DEFAULT_REGION: ${{ secrets.AWS_DEFAULT_REGION }}
      AWS_BACKUP_BUCKET: ${{ secrets.AWS_BACKUP_BUCKET }}
      BACKUP_ARCHIVE_PASSWORD: ${{ secrets.BACKUP_ARCHIVE_PASSWORD }}
```

## `laravel-pint-fixer.yml`

Workflow to run [Laravel Pint](https://github.com/laravel/pint) and automatically fix code style violations. Changes are pushed back to the GitHub repository.

```yml
# .github/workflows/laravel-pint-fixer.yml
name: Laravel Pint

on:
  pull_request:
  push:
    branches:
      - main

jobs:
  pint:
    uses: stefanzweifel/reusable-workflows/.github/workflows/laravel-pint-fixer.yml@main
```

## `php-cs-fixer.yml`

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

## `phpstan.yml`

Workflow to run [PHPStan](https://phpstan.org/) in projects.
To prevent usage spikes, the job is not executed if the user is Dependabot.

```yml
# .github/workflows/phpstan.yml
name: PHPStan

on:
  push

jobs:
  update_release_draft:
    uses: stefanzweifel/reusable-workflows/.github/workflows/phpstan.yml@main
    with:
      php_version: '8.3'
```

## `post-release-comments.yml`

This workflow uses [Duncan McClean](https://github.com/duncanmcclean/)'s [post-release-comments](https://github.com/duncanmcclean/post-release-comments) action to notify users in issues and pull requests, if their issue or pull request has been mentioned in a release.

```yml
# .github/workflows/post-release-comments.yml
name: Post Release Comments

on:
  release:
    types: [released]

jobs:
  comments:
    uses: stefanzweifel/reusable-workflows/.github/workflows/post-release-comments.yml@main
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
  update:
    uses: stefanzweifel/reusable-workflows/.github/workflows/update-changelog.yml@main
```
