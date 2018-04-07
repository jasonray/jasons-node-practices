# jasons-npm-practices

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

For me, I use the following practice when updating:
- Start from a clean branch (such as `next` or a branch off your working director)
- Make sure that your dependencies are installed (`npm rebuild`)
- Run my unit/integration tests and see that all is green
- Identify one module that needs to be updated by running `npm-check`
- Update that one module using the command from npm-check
- Re-run my unit/integration tests and see that all is still green.  Resolve any issues.
- Commit the changes, which will, at a minimum include `package.json`.  Favor a single commit per module upgrade.  Favor a commit message that state the new version (`upgrade yargs to 11.0.0`).
- Upgrade additional modules
- Release new version or merge branch*

*: On small projects, I favor releasing upgrades on their own as a patch release.  On large project, or if I discover the need to upgrade while in the middle of another release, I often lazily upgrade with the other planned release.
