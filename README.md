# jasons node/npm practices

This page has some of my standard best practices while I am working on node projects.

# Keep dependencies up to date.

It is easy to let dependencies get stale.  I use npm-check [https://www.npmjs.com/package/npm-check].  
Install it:
```
$ npm install -g npm-check
```

Run it:
```
$ npm-check
```

## Execute update

For me, I use the following practice when updating:
- Start from a clean branch (such as `next` or a branch off your working directory)
- Make sure that your dependencies are installed (`npm rebuild`)
- Run my unit/integration tests and see that all is green
- Identify one module that needs to be updated by running `npm-check`
- Update that one module using the command from npm-check
- Re-run my unit/integration tests and see that all is still green.  Resolve any issues.
- Commit the changes, which will, at a minimum include `package.json`.  Favor a single commit per module upgrade.  Favor a commit message that state the new version (`upgrade yargs to 11.0.0`).
- Upgrade additional modules
- Release new version or merge branch*

*: On small projects, I favor releasing upgrades on their own as a patch release.  On large project, or if I discover the need to upgrade while in the middle of another release, I often lazily upgrade with the other planned release.

# Versioning

- Merge master into working directory (`next`)
- Branch from working directory into feature branches, and merge back to working directory as complete
- When ready to release..
- Branch to release branch.  Name after targeted npm version, in format `release-v1.2.3`.  Example: `git checkout -b release-v1.2.3`
- Run your unit tests make sure it is good Example: `npm test`
- bump version in npm, and create git tag.  Example: `npm version minor -m "bump version to 1.6"`
- Publish module.  `npm publish`
- push tag to origin `git push origin v1.6.0`
- Mark github release.  Access project in github, click `releases`, click `tags`, to right of tag, click `create release`
- merge to next, master

# Plug-ins / badges

## Travis-CI (CI)

Travis-CI is a powerful / free build environment.  It is great for running a build when you push to select branches, pull requests are formed, or on a schedule (like daily/monthly).  It can run matrixed environments (like different versions of node or OS).  Not bad for free.

[Overview](https://docs.travis-ci.com/user/tutorial/)

First you need some one time account setup stuff to be done.  I will not cover that here.

Add a `.travis.yml` to the root of your git repo.  This provides the instructions for your build job.

Here is a pretty basic version:
```
language: node_js
sudo: false
node_js:
  - "node"
  - "8"
```

Here is a more complex one from one of my projects.  A few things to note in here:
- language is used to specify java, ruby, etc.  This is a node project
- node_js specifies I want to run with version 8 and the current (node)
- addons.apt is used to install OS level packages
- env in this case is used to specify different env variables (I use it here so that I can run once with unit tests and one with integration tests)
- full list of [options can be found in documentation here](https://docs.travis-ci.com/user/customizing-the-build)

```
language: node_js
sudo: false
node_js:
  - "node"
  - "8"

addons:
  apt:
    packages:
    - beanstalkd

env:
- TEST_CMD=test
- TEST_CMD="run integration-test"

before_install:
- echo 'starting queue'
- bin/start-queue.sh &
- jobs -l

after_script:
- npm install -g npm-check
- npm-check

script:
- npm $TEST_CMD
```

### Travis-CI badge

Get the badge from your [Travis-CI](https://travis-ci.org/jasonray) build page.  This will list the current build status on your repo readme

## NPM version

## Coveralls (code coverage)

## Greenkeeper (help keep current version)

