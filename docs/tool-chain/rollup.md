# 制造rollup

## 前端模块化规范
- CommonJS
- AMD
- UMD
- ESM



## 模块打包工具rollup


### 打包格式
amd
cjs
esm
iife
umd

```bash
./node_modules/.bin/rollup -i src/index.js -o dist/bundle.es.js -f es
```



`rollup.config.js`

```js
export default {
  input: "./src/index.js",
  output: {
    file:"./dist/es.min.js",
    format: "esm"
  }
}
```



## 模块

```js
class Module {
  constructor({code, path, bundle}) {

  }
}


```



## 打包


`rollup/lib/bundle.js`

```js
const path = require('path'')
class Bundle {
  constructor({entry}){
    // ext less
    this.entryPath = entry.replace(/\.js$/,'') + '.js'
    this.entryName=
  }
  /**
    *
    * @param importee
    * @param importer
  */
  fetchModule(importee, importer) {


    route = path.resolve(
      path.dirname(importer),
      importee.replace(/\.js$/,'') + '.js'
    )

    code = fs.readFileSync(route, 'utf-8').toString()
  }
  generate() {
    if ( statement.type = "ExportNameDeclaration") {
      source.remove(statement.start, statement.delcaration.start)
    }
  }
}

module.exports = Bundle
```
`rollup/lib/__test__/bundle.spec.js`
```js
const Bundle = require('../bundle')

describe("bundle test", () => {
  test("fecthModule", ()=> {
    const bundle = new Bundle({entry:'./.js'))
    fs.readFileSync.mockReturnValueOnce()
    bundle.fetchModule()
  })
})
```
