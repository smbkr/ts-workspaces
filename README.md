# Lerna

## Commands

`npx lerna init` start a new project or convert existing. can then just use `lerna` once it's in /node_modules/.bin

or install globally `npm i -g lerna`

`lerna bootstrap` - install dependencies and link local dependencies
`lerna clean` - delete dependencies
`lerna create` - add new module

`lerna run build --stream` sequential
`lerna run test --parallel` self explanatory

`lerna run` will run the corresponding script for each package, e.g. `lerna run test` looks for `test` script in
package.json in each package. doesn't fail if the script doesn't exist, allows omitting `build` for example for libs
that are not build to distribution but just included elsewhere

## Notes

to use with yarn/workspaces

```
// lerna.json
+  "npmClient": "yarn",
+  "useWorkspaces": true
```

all workspaces/packages in `./packages/foo`
subdirectories seem to work, e.g. /packages/lib/logger /packages/service/foobar

although it's easier if they are just in /packages/whatever, flat means less config

`npx lerna bootstrap` for initial install

then `lerna [whatever]` after

https://medium.com/ah-technology/a-guide-through-the-wild-wild-west-of-setting-up-a-mono-repo-with-typescript-lerna-and-yarn-ed6a1e5467a

command.version.ignoreChanges: an array of globs that won't be included in lerna changed/publish. Use this to prevent
publishing a new version unnecessarily for changes, such as fixing a README.md typo

```json
{
  "packages": [
    "packages/*"
  ],
  "npmClientArgs": [
    "--no-lockfile"
  ],
  "version": "independent",
  "command": {
    "version": {
      "ignoreChanges": [
        "*.md"
      ],
      "npmClient": "npm",
      "message": "chore(release): publish"
    },
    "publish": {
      "npmClient": "npm"
    }
  }
}
```

---

## TypeScript

tsconfig.json in the root defines the paths for packages we want to be able to import elsewhere, e.g. to allow service '
foobar' to import the logger

tsconfig-build.json is our base config (could have called it tsconfig-base.json or whatever. Not sure why I put build?).
It is extended by each service's tsconfig, and defines the common config like target lib, module resolution options,
etc.

Each package's tsconfig is like the below:

```
{
  "extends: "../../tsconfig.build.json",
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist"
  },
  "include": [
    "src/**/*"
  ]
}
```

For each package's tsconfig, we have to declare the rootDir, outDir and include properties. They can’t be hoisted in the
root config because they are resolved relative to the config they are in.

**TODO:**

it would be better if we didn't need an explicit entry for every package in the root tsconfig.json so that packages just
got detected automatically.

## packages

### common

exports a string

should be importable by other packages

### foobar

like a pretend service, imports the logger and the string from common and logs the string

### logger

exports a logger function, should be importable everywhere