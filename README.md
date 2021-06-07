![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/ashimjk/conventional-commit-tryout)

# Conventional Commit Tryout

## Startup
- [Semantic Versioning](https://semver.org/)
- [Conventional Commit](https://conventionalcommits.org)
- [Semantic Release - Tool](https://github.com/semantic-release/semantic-release)
- [Standard Version - Tool](https://github.com/conventional-changelog/standard-version)
- [Conventional Github Releaser](https://github.com/conventional-changelog/releaser-tools/tree/master/packages/conventional-github-releaser)
- [Conventional Commit for Maven](https://github.com/dwmkerr/standard-version)
- [Create Semantic Module](https://github.com/jlegrone/create-semantic-module)
- [Husky](https://typicode.github.io/husky/#/)

---

## Conventional Commit
Please go through the documentation for [more information](https://conventionalcommits.org).

According to Conventional Commit, a commit message should be structured as follows.

```
<type>[(optional scope)]: <description>

[optional body]

[optional footer(s)]
```

### Type
- `feat` ~~> addition of some new features
- `add` ~~> changes to add new capability or functions
- `cut` ~~> removing the capability or functions
- `fix` ~~> a bug fix
- `bump` ~~> increasing the versions or dependency versions
- `build` ~~> changes to build system or external dependencies
- `make` ~~> change to the build process, or tooling, or infra
- `ci` ~~> changes to CI configuration files and scripts
- `doc` ~~> changes to the documentation
- `test` ~~> adding missing tests or correcting existing tests
- `chore` ~~> changes for housekeeping (avoiding this will force more meaningful message)
- `refactor` ~~> a code change that neither fixes a bug nor adds a feature
- `style` ~~> changes to the code that do not affect the meaning
- `optimize/perf`: a code change that improves performance
- `revert` ~~> reverting an accidental commit

### Scope
- Must not contain a periods(.) at the end
- Must not capitalize the first letter
- Do not use issue identifiers as scopes
- Use imperatives

### Footer
Used for:
- referencing breaking changes
- sign-offs
- tracking issue references
- author information

### Breaking Changes
- a commit that has a footer `BREAKING CHANGE:`
- appends a `!` after the type/scope, introduces a breaking API change (correlating with MAJOR in semantic versioning).
- A BREAKING CHANGE can be part of commits of any type. This is also considered a special footer.

```example
## Commit message with description and breaking change footer

feat: allow provided config object to extend other configs

BREAKING CHANGE: `extends` key in config file is now used for extending other config files

## Commit message with ! to draw attention to breaking change

refactor!: drop support for Node 6
```

---

## [Commitizen](http://commitizen.github.io/cz-cli/)
It is a tool to create a commit message. It will prompt you, to fill out any required commit fields.
After that it will create a commit message based on conventional commit in formatted way.

`cz-conventional-changelog` is adapter to format the message.

### Installation
```shell
npm install --save-dev commitizen
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

### How to use
We can `npx cz` command for git commit. It will ask a set of questions and after we fill it out,
it will commit with proper message.

NOTE: Currently it doesn't support multi-line description, and to use it we need to provide
`\n` as new line. Although it bit tedious, but will do the job.

---

## [Commitlint](https://commitlint.js.org/#/)
Tool to lint the commit message

### Installation
```shell
npm install --save-dev @commitlint/{cli,config-conventional}
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

### How to use
```shell
echo 'foo: bar' | npx commitlint

# Lint last commit from history
npx commitlint --from=HEAD~1
```

## Git Hook - Commit Message
Now, we can use `commitlint` to check whether commit message is valid or not.
And to trigger it, we need to use `commit-msg` git hook.

`commit-msg` hook get executed after the user has entered the message and before finishing the commit.
If this hook exists with a status other than 0, then commit fails.

We can copy one of the sample file and paste it to `.git/hooks` directory. Also we need to make commit-msg
script executable. To do that we can use `chomd +x .git/hooks/commit-msg`.

NOTE: To make `npx` available from anywhere, we need to use `$PATH` variable in the script.

### Sample
These sample are nothing but plan bash script.

- [Commit Message Sample 1](commit-msg-sample-1)
- [Commit Message Sample 2](commit-msg-sample-2)

---

## Git Hook - Using Husky

### Setup
```shell
# Install
npm install husky --save-dev

# Enable Git Hooks
npx husky install

# To automatically have Git hooks enabled after install
npm set-script prepare "husky install"

# Add Hook
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
```

After the setup, any invalid commit message will be blocked. And error message will be shown
along with explanation.

---

## Conventional Changelog CLI
Generate a changelog from git metadata

```shell
# Install
npm install conventional-changelog-cli --save-dev

# Generate change log
npx conventional-changelog

# Generate change log using angular preset
npx conventional-changelog -p angular

# For first time
npx conventional-changelog -p angular -i CHANGELOG.md -s -r 0

# For others
npx conventional-changelog -p angular -i CHANGELOG.md -s
```

We can also use `npm version [patch|minor|major]` command to bump the version and push the changes.
We can add following command in `.npmrc` to format the commit message.

```
tag-version-prefix="v"
message="chore(release): v%s"
```

For more information [follow the link](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli#with-npm-version)

---

## Conventional Github Releaser
Make a new GitHub release from git metadata.

First, we need to [create a new token](https://github.com/settings/tokens/new) and that token should have the scope `public_repo` (or `repo` for access to private repositories). Now, we can follow below steps to setup the github-release.

```shell
# Install
npm install conventional-github-releaser --save-dev

# Generate release
npx conventional-github-releaser -t token

# Generate release using angular preset
conventional-github-releaser -p angular -t token
```

---

## Workflow
- Make changes
- Commit those changes
- Bump version using `npm version [major|minor|patch]`
  - It will update the version in `package.json`
  - Create a change log
  - Commit the changes and create tag
- Push the changes
- Create github release using `npm run github-release`

---

## Modules Important to Conventional Changelog Ecosystem
- [conventional-changelog-cli](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli) - the full-featured command line interface
- [standard-changelog](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/standard-changelog) - command line interface for the angular commit format.
- [conventional-github-releaser](https://github.com/conventional-changelog/conventional-github-releaser) - Make a new GitHub release from git metadata
- [conventional-recommended-bump](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-recommended-bump) - Get a recommended version bump based on conventional commits
- [conventional-commits-detector](https://github.com/conventional-changelog/conventional-commits-detector) - Detect what commit message convention your repository is using
- [commitizen](https://github.com/commitizen/cz-cli) - Simple commit conventions for internet citizens.
- [commitlint](https://github.com/conventional-changelog/commitlint) - Lint commit messages
