> This branch contains a fixed package-lock.json, which is the cause of the error in the main branch. Note that even though the `dependency` package doesn't exist anymore (it was renamed to `greeting`), `npm install` is not able to tell that there is a problem, until we try to run it with `--workspace`. This seems like a bug, but one where `npm` should fail more often, not less, and give an appropriate error.

Minimal reproduction for https://github.com/npm/cli/issues/3847 without any
`npm link` or `file:` references, just a plain NPM workspace referring to other
packages by package name:

```console
$ # Reproduces with all the following:

$ nvm use 18
Now using node v18.3.0 (npm v8.11.0)

$ nvm use 17.2.0
Now using node v17.2.0 (npm v8.1.4)

$ nvm use 17
Now using node v17.9.1 (npm v8.11.0)

$ nvm use 16
Now using node v16.14.2 (npm v8.5.0)

$ rm -rf node_modules && npm install --workspace packages/server
npm ERR! Cannot set properties of null (setting 'dev')

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/.../.npm/_logs/...-debug.log

$ rm -rf node_modules && npm install

added 3 packages, and audited 8 packages in 130ms

found 0 vulnerabilities
```
