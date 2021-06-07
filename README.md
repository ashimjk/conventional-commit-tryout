# Conventional Commit Tryout

## Startup
- [Semantic Versioning](https://semver.org/)
- [Conventional Commit](https://conventionalcommits.org)
- [Semantic Release - Tool](https://github.com/semantic-release/semantic-release)
- [Standard Version - Tool](https://github.com/conventional-changelog/standard-version)
- [Conventional Github Releaser](https://github.com/conventional-changelog/releaser-tools/tree/master/packages/conventional-github-releaser)


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

