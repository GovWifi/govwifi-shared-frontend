# GovWifi Shared Frontend

## Description
This package offers shared functionality across the three websites that make GovWifi.

`govwifi-shared-frontend` is used by:
* [`govwifi-admin`](https://github.com/govwifi/govwifi-admin)
* [`govwifi-product-page`](https://github.com/govwifi/govwifi-product-page)
* [`govwifi-tech-docs`](https://github.com/govwifi/govwifi-tech-docs).

## Usage
- `npm install govwifi-shared-frontend`;
- include the file `dist/govwifi-shared-frontend.js`: it will install
  itself in `window.GovWifi`;
- Add a page load event listener and plug the API described below.

## API

### Cookies
Cookies functions are scoped to `GovWifi.cookies`.

#### `GovWifi.cookies.checkCookiePolicy()`
Checks whether cookie policy is defined and prompts the user with a
dialog to define it if not. The dialog will prepend a
`div#cookie-banner` node to the document's body.

#### `GovWifi.cookies.isCategoryAllowed(categoryName)`
Indicate whether the user has agreed to that category of cookies. The
only category at the moment is `analytics` (and `essential`, but that
will always return true).

#### `GovWifi.cookies.setCategoryAllowed(categoryName, isAllowed)`
Amend the cookie policy to allow or disable the given
`categoryName`. If `categoryName` is not recognised do nothing.

## Release process

### Update the code

1. Create a branch from the main branch and make any necessary code or library updates.

2. If you’ve updated any libraries run `npm install`.

    This will update the `package-lock.json` file which also must be committed.

3. Comment and push the code to a feature branch, please note, the comment will be used in the release notes, please **Do Not** start the comment with the version eg 0.6.xx, instead use something like "Bumped xx library to latest version".

4. don't forget to **Update the artifact version** below.

### Update artifact version

After all updates are committed and pushed, update the version with the semantic version (major|minor|patch),
[For more information review semantic versioning docs](https://semver.org/), eg.

    ```bash
    $ npm version patch
    ```

Which will update the version in package.json, which will then be committed and pushed to the branch.

### Release Artifact

1. Raise a Pull Request. Once approved, merge the PR into the main branch.

2. Once merged the GitHub Action will create a release version and tag for the artifact, to use in the **Update GovWifi repos** step, to update the repos that rely on this artifact.

### Update GovWifi repos

Once the new version of the project has been released in GitHub, we need to update the `package.json` files in the GovWifi repos which use `govwifi-shared-frontend`.

* `govwifi-admin`
* `govwifi-product-page`
* `govwifi-tech-docs`

For each of these projects, complete the following steps:

1. Create a new branch `update-govwifi-shared-frontend` from the main branch.
2. In the repo's `package.json`, update the "govwifi-shared-frontend" dependency to point to the new release version:
    ```json
      "dependencies": {
        ...
        "govwifi-shared-frontend": "https://github.com/govwifi/govwifi-shared-frontend/releases/download/v{release-version}/govwifi-shared-frontend-{release-version}.tgz"
      },
    ```
   Use the link address of the `.tgz` file found in the "Assets" section of the `govwifi-shared-frontend` ["Releases"](https://github.com/govwifi/govwifi-shared-frontend/releases) page.
3. Run `npm install` to pull in the new release.
4. Test the app locally to see if the update has caused any breaking changes.
5. Commit the changes (this should just be `package.json` and `package-lock.json`) and raise a PR.
6. Once the PR is merged follow the stated deployment process for the repo.

## TODO

- [ ] add tests

### Manual Release process
If for any reason it's required to build and release manually, follow these steps
#### To build release package - manual release process only.

The release package must be uploaded to GitHub as part of the release process so it can be downloaded by the repos which use `govwifi-shared-frontend`.

1. Run the following from the root project directory:

    ```bash
    $ npm run build
    ```

    This creates a “distribution” folder or `dist`. The `dist` contents are configured via webpack in the [`webpack.config.js`](webpack.config.js) file.
    Inside this folder will be the generated compressed distribution package,  with a naming structure like, `{name}-{release-version}.tgz`, of the JS code which will need to be
     upload to GitHub:

#### Update Github release version - manual release process only.

Navigate to the `govwifi-shared-frontend` ["Releases"](https://github.com/govwifi/govwifi-shared-frontend/releases) page.

Click ["Draft new release"](https://github.com/govwifi/govwifi-shared-frontend/releases/new), then follow the release version process:

1. Under "Choose tag", create a new tag for the release or use an existing tag if it's appropriate.
2. Use the release version number for the "Release title"
3. Add a useful description of the changes in the release, including links to Dependabot PRs if applicable.
4. Click on "Attach binaries by dropping them here or selecting them."
5. Attach the `govwifi-shared-frontend-{release-version}.tgz` file created earlier, found in the Dist folder.

