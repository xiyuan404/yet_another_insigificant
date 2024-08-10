# npm cli

## cli commands

installed package and register to a package.json file as dependencies
`npm install --save-prod`
`npm install --save-dev`


### npm link


- [x] `npm link`

1.`npm link` in a package folder with no arguments will create a symlink in the global folder {prefix}/lib/node_modules/<package> that links to the package where the npm link command was executed.
2. it will also link any bins in the package to {prefix}/bin/{name}. Note that npm link uses the global prefix (see npm prefix -g for its value).

```sh
➜  bin ls -l Users/ayao/.nvm/versions/node/v22.4.1/bin/
quarte -> ../lib/node_modules/@quarte/cli/bin/cli.js
```

### packageManager

```json
{
  "packageManager": "pnpm@9.6.0"
}

```



### Workspace Usage

`npm link <pkg> --workspace <name>`

`npm link --workspace <name>`

### enterprise practice

```json
{
  "worksapces": {
    "packages/@vue/*",
    "packages/test/*",
    "packages/vue-cli-version-marker"
  }
}
```


## .npmrc





## semver

## npm install

npm install saves any specified packages into dependencies by default. Additionally, you can control where and how they get saved with some additional flags:

         •   -P, --save-prod: Package will appear in your dependencies. This is the default unless -D or -O are present.

         •   -D, --save-dev: Package will appear in your devDependencies.

         •   -O, --save-optional: Package will appear in your optionalDependencies.

         •   --no-save: Prevents saving to dependencies.


## npm配置文件字段解析




### imports

- [x] built-in lib imports

```json
{
  "imports": {
    "#ansi-styles": "./source/vendor/ansi-style/index.js",
    "#supports-color": {
      "node": "./source/vendor/supports-color/node.js",
      "browser": "./source/vendor/supports-color/browser.js"
    }
  }
}

```


### scirpts

node_modules/.bin

```json
{
  "sciprts": {
    "build": "webpack"
  }
}
```
```sh
npx webpack
```

## 包依赖管理
`dependencies`
`devDependencies`
`peerDependencies`


## Semantic Versioning Specification


### document
![](https://semver.org/)
consider当前版本`1.0.0`,发布更新
```sh
patch (1.0.1) #make backward compatible bug fixes
Minor (1.1.0) # add functionality in a backward compatible manner
Major (2.0.0) # when you make incompatible API changes
---- prelease ----
Prepatch (1.0.0-alpha.0)
Preminor (1.1.0-alpha.0)
Premajor (2.0.0-alpah.0)

```

#### rapid development and fast iteration
start your initial development release at 0.1.0 and then increment the minor version for each subsequent release.

!!! note
- we propose a simple set of rules and requirements that dictate how version numbers are assigned and incremented.

- Consider a version format of X.Y.Z (Major.Minor.Patch). Bug fixes not affecting the API increment the patch version, backward compatible API additions/changes increment the minor version, and backward incompatible API changes increment the major version.

- Once a versioned package has been released, the contents of that version MUST NOT be modified. Any modifications MUST be released as a new version.

### foramt specification

```BNF
<valid semver> ::= <version core>
                 | <version core> "-" <pre-release>
                 | <version core> "+" <build>
                 | <version core> "-" <pre-release> "+" <build>
```

### node-semver implementation
`bin/semver.js`
```js
const argv = process.argv.slice(2)


const versions = []

const main = () => {
  if(argv.length) return help()
  // normalizeCommandLineOptions or Command-line argument parsing
  while(argv.lenght) {
    // --version=1.0.0
    let option = argv.shift()
    const idxOfEqualSign = option.indexOf("=")
    if(idxOfEqualSign > -1) {
      const val = option.slice(idxOfEqualSign + 1)
      option = option.slice(0,idxOfEqualSign)
      argv.unshift(val)
    }
  }

  switch(option) {
    case '-v': case '--version':
      versions.push(argv.shift())
      break
  }
}

const help = () => console.log(
  `
  A JavaScript implementation of the https://semver.org/ specification

  Usage: semver [options] <version> [<version> [...]]

  Options:
  -r --range <range>
          Print versions that match the specified range
  `
)

main()
```


## multi-package management

> Workspaces is a generic term that refers to the set of features in the npm cli that provides support to managing multiple packages from your local files system from within a singular top-level, root package.


### configure workspace

"upward lookup": from the currnet directory, looking for a directory containing `node_modules` directory

`npm init --workspace`: get symlinked to the `node_modules` folder of the current working dir.


```json
{
  "workspaces": ["./packages/*"]
  "publishConfig": { // when using a scope,
    "access": "public"
  },
  "bin": {
    "4am": ".src/cli.js" // bin Package has an executable.
  }
}
```


### add dependencies to a specific (package/workspace)

` npm i import-local -w packages/cli --include-workspace-root `s


### Running commands in the context of workspaces


`npm run test --workspaces --if-present`


### cache



### scope

- [x] Installing scoped packages

scoped modules will be installed in `node_modules/@myorg/pkgnmae`

A scoped package is installed by referencing it by name, preceded by an @ symbol, in npm install:


- [x] Requiring scoped packages

you have to include the name of the scope when requiring them in your code

- [x] Publishing scoped packages


Scoped packages are not public by default. You will need to specify --access public with the initial npm publish command.
or set access to public `publishConfig.access` file in `package.json`

> URL-safe characters
> All npm packages have a name. Some package names also have a scope
> scopes are preceded by an @ symbol and followed by a slash



## how npm handle script field

### Pre & Post Scripts

`npm run <script>` find exeuctable in `node_modules/.bin` and run
Pre and post commands with matching names will be run for those as well


### file field in package.json

describes the entries to be included when your package is installed as a dependency
Certain files are always included, regardless of settings:
- package.json
- README
- LICENSE / LICENCE
- The file in the "main" field
- The file(s) in the "bin" field
