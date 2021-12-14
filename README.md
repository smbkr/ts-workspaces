# README

Useful reading:
https://medium.com/ah-technology/a-guide-through-the-wild-wild-west-of-setting-up-a-mono-repo-with-typescript-lerna-and-yarn-ed6a1e5467a

## Yarn 2

https://yarnpkg.com/getting-started/install
```
nvm use
corepack enable
yarn init -2
yarn set version stable
```


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

## TODO

make a new branch and get rid of lerna, see if we can manage the build process above with just yarn

maybe try yarn 2?
