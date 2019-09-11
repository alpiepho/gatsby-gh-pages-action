# Gatsby Publish

GitHub Action to build and deploy your Gatsby site to GitHub Pages ❤️🎩

## Usage

This GitHub Action will run `gatsby build` at the root of your repository and
deploy it to GitHub Pages for you! Here's a basic workflow example:

```yml
name: Gatsby Publish

on:
  push:
    branches:
      - dev

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: enriikke/gatsby-gh-pages-action@v2
        with:
          access-token: ${{ secrets.ACCESS_TOKEN }}
```

### Knobs & Handles

This Action is fairly simple but it does provide you with a couple of
configuration options:

- **access-token**: A [GitHub Personal Access Token][github-access-token] with
  the `repo` scope. This is **required** to push the site to your repo after
  Gatsby finish building it. You should store this as a [secret][github-repo-secret]
  in your repository. Provided as an [input][github-action-input].

- **deploy-branch**: The branch expected by GitHub to have the static files
  needed for your site. For org and user pages it should always be `master`.
  This is where the output of `gatsby build` will be pushed to. Provided as an
  [input][github-action-input].
  Defaults to `master`.

- **gatsby-args**: Additional arguments that get passed to `gatsby build`. See the
  [Gatsby documentation][gatsby-build-docs] for a list of allowed options.
  Provided as an [input][github-action-input].
  Defaults to nothing.

### Org or User Pages

Create a repository with the format `<YOUR/ORG USERNAME>.github.io`, push your
Gatsby source code to a branch other than `master` and add this GitHub Action to
your workflow! 🚀😃

### Repository Pages

Repo pages give you the option to push your static site to either `master` or
`gh-pages` branches. They also work a little different because the URL includes
a trailing path with the repository name, like
`https://username.github.io/reponame/`. You need to tell Gatsby what the path
prefix is via `gatsby-config.js`:

```js
module.exports = {
  pathPrefix: "/reponame",
}
```

Additionally, you need to tell the `gatsby build` command to use it by passing
the `--prefix-paths` as an argument. Here's an example workflow for that:

```yml
name: Gatsby Publish

on:
  push:
    branches:
      - dev

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: enriikke/gatsby-gh-pages-action@v2
        with:
          access-token: ${{ secrets.ACCESS_TOKEN }}
          deploy-branch: gh-pages
          gatsby-args: --prefix-paths
```

### Assumptions

This Action assumes that your Gatsby code sits at the root of your repository
and `gatsby build` outputs to the `public` directory. As of this writing, Gatsby
doesn't provide a way to customize the build directory so this should be a safe
assumption.

## That's It

Have fun building! ✨

[gatsby-build-docs]: https://www.gatsbyjs.org/docs/gatsby-cli/#build
[github-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line
[github-action-input]: https://help.github.com/en/articles/workflow-syntax-for-github-actions#jobsjob_idstepswith
[github-repo-secret]: https://help.github.com/en/articles/virtual-environments-for-github-actions#creating-and-using-secrets-encrypted-variables
