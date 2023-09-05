<p>
  <a href="https://github.com/helsingborg-stad/municipio-deploy/release-wp-plugin">
    <img src="./images/hbg-github-logo-combo.png" alt="Logo" width="300">
  </a>
</p>

# Release WP Plugin Action

This action will calculate the new version number of a WordPress plugin and create a new release on Github with the new version number and the plugin zip file as an asset.

The action also sets the same version number in the composer.json, package.json and the plugin php file if available in the plugin repository root.

## Parameters

| Parameter name              | Description                                                                  | Default    | Required |
|-----------------------------|------------------------------------------------------------------------------|------------|----------|
| php-version                 | PHP Version                                                                  | 7.4        | false    |
| node-version                | NodeJS Version                                                               | 16.13.2    | false    |

## Requirements
- A `GitVersion.yml` must be present in the root of the repository. You can read more on how to configure GitVersion [here](https://gitversion.net/docs/configuration/). If you aim to use Conventional Commits you can use the following configuration in your GitVersion.yml file:

```
mode: MainLine
major-version-bump-message: "^(build|chore|ci|docs|feat|fix|perf|refactor|revert|style|test)(\\([\\w\\s-]*\\))?(!:|:.*\\n\\n((.+\\n)+\\n)?BREAKING CHANGE:\\s.+)"
minor-version-bump-message: "^(feat)(\\([\\w\\s-]*\\))?:"
patch-version-bump-message: "^(build|chore|ci|docs|fix|perf|refactor|revert|style|test)(\\([\\w\\s-]*\\))?:"

branches:
  main:
    regex: ^main$|^master$
    is-release-branch: true
```

Please note that you may need to update the "branches" attribute to match your main branch name.

## Usage
In your WordPress plugin repository, create a file in `.github/workflows/` named ex. `release.yml`.:
- Use the following template.
```
name: Bump version and create release

on:
  push:
    branches: [ branchname ]

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
    
    - name: Checkout
      uses: actions/checkout@v2
      with:
        fetch-depth: 0

    - name: Create Release and bump version files
      uses: helsingborg-stad/release-wp-plugin-action@1.0
      with:
        php-version: 8.2
        node-version: 20.6.0
  ```
- Note that the action is using the `actions/checkout@v2` action to checkout the repository. This means that you need to make sure that the branch you want to deploy is checked out.
- Make sure to update the `php-version` and `node-version` to match the versions you want to use, or remove them to use defaults.
- Make sure branchname is matching the branch you would want to deploy.
- Replace secret names with macthing secrets.
