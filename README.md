version bumper
==============

A set of npm scripts to automate the process of version releases

Install
-------

This proof of concept will make commits to its repo and will try to push
those commits to its remotes.

To avoid making a mess in the original repository, **first fork this repo**

Then

    git clone <registry/npm-version-bumper>
    cd npm-version-bumper
    npm install

Usage
-----

### Fixed version

    npm run version -- 2.5.0

  * no commit
  * no tag

As of `npm@2.0`, npm run scripts can receive parameters following a `--`
delimiter indicating the end of npm params and the beginning of task params

    npm run version -- --prerelease

Non-dasheed arguments (like the version number `2.5.0`) doesn't seem to require
the `--` delimiter

    npm run version 2.5.0

### Release

    npm run version-release

  * current version: `2.5.1`
  * commit: `Release 2.5.1`
  * tag: `2.5.1`

### Development

    npm run version-devel

  * current version: `2.5.1-0`
  * commit: `Preparing for next development iteration (2.5.1-0)`
  * no tag

### Bump

    npm run version

Shortcut for the most common use case:

 1. `npm run version-release`
 2. `npm run version-devel`

Design decisions
----------------

`mversion` package is used instead of `npm version` to be able to bump several
files at once: `package.json` and `bower.json` in our case

~~`.mversionrc` file contains a hook to push the changes after a commit is done
by `mversion`~~

**[upd]** complex tasks are stored as shell scripts at `bin/`.  git operations
are specified in those scripts

`npm run broadcast` pushes changes and tags to all available git remotes

`npm run version-current` is needed to get an updated version in the environment
variable

~~`npm run version-devel` uses git directly to commit the changes instead of
using `mversion` to avoid creating a `git tag` for that commit~~

**[upd]** npm run scripts are used for 2 main use cases:

  * aliasing node CLI scripts to avoid global installations
  * aliasing scripts located at `bin/` to avoid path mistakes, `pwd`, ...

To Do
-----

Rewrite `bin/` scripts in `nodejs` for better compatibility
(`shelljs` comes handy for those scripts)

References
----------

  * <https://docs.npmjs.com/cli/versio://docs.npmjs.com/cli/version>
  * <http://stackoverflow.com/questions/28421489/what-is-the-convention-for-versioning-npm-packages-prior-to-1-0-0>
  * <http://carrot.is/coding/npm_prerelease>
  * <https://github.com/mikaelbr/mversion>
  * <https://github.com/npm/npm/pull/5518>
  * <https://github.com/npm/npm/issues/6053>
  * <http://stackoverflow.com/questions/10972176/find-the-version-of-an-installed-npm-package>
  * <https://docs.npmjs.com/misc/scripts#package-json-vars>
