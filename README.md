# Lerna

## Commands

`npx lerna init` start a new project or convert

`npx lerna-wizard`

`lerna bootstrap` - install dependencies and link local dependencies
`lerna clean`
`lerna create` - add new module

## Notes

to use with yarn/workspaces

```
// lerna.json
+  "npmClient": "yarn",
+  "useWorkspaces": true
```

all workspaces/packages in `./packages/foo`
subdirectories seem to work, e.g. /packages/lib/logger
/packages/service/foobar

`npx lerna bootstrap` for initial install

then `lerna [whatever]` after

