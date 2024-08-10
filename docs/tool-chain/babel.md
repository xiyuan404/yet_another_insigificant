- babel的安装和使用


## babel的命令行使用(via cli)

### 插件的使用
```sh
npm i @babel/core @babel/cli -D
npm i @babel/plugin-transform-block-scope @babel/transform-arrow-functions -D
```
```sh
npx babel ./src --out-dir ./dist --plugins=@babel/plugin-transform-block-scope,@babel/plugin-transform-arrow-functions
```

### 预设


```sh
npm i @babel/preset -D
npx babel ./src --out-dir --presets=@babel/preset
```

### babel as a compiler
[AST Explorer](https://astexplorer.net/)
source code -> lexical analysis -> tokens
tokens -> syntactic analysit -> AST
AST -> Traversal -> Vistor -> Plugin -> new AST
new AST -> code generation -> target code

##  babel在webpack配置文件中的使用(w/ configuration file)
babel as a loader
installattion
```sh
npm install babel-loader @babel/core
```


```js title="webpack.config.js"
module.exports = {
  devtool: 'cheap-source-map',
  output: {
    path:path.resolve(__dirname,'./build'),
    filename:'bundile.js',
    clean: true
  },
  reslove:{
    extensions:['.js','json','.jsx']
  },
  module:{
    rules: [
      {
        test:/\.ts$/,
        exclude: /node_modules/,
        use:'ts-loader'
      },
      {
        test: /\m?.js/,
        // loader:'babel-loader'
        exclude:/node_modules/,
        use: {
          loader:'babel-loader',
          options: {
            // plugins: [
            //   "@babel/plugin-transform-arrow-functions",
            //   "@babel/plugin-transform-destructuring",
            //   "@babel/plugin-transform-for-of"
            // ]
            presets:[
              [
                "@babel/preset-env",
                {
                  targets: {
                    "edge": "17","firefox": "60", "chrome": "67",        "safari": "11.1"
                  }
                }
              ],
              [
                "@babel/preset-ract"
              ]
          }
        }
      }
    ]
  }
}
```

### babel-plugins


### babel-preset

webpack提供一个预设(preset),babel-loader会根据我们提供的预设加载包含(included)在该预设中的插件，传递给`@babel/core`中的compiler

常见的预设有三个:
`@babel/preset-env`
`@babel/preset-react`
`@babel/preset-typescript`

[TC-39 process](https://tc39.es/process-document/)
stage-0: Strawman
stage-1: Proposal
stage-2: Draft
stage-3: Candidate
stage-4: Finished

### 浏览器兼容性
打包的代码，最终要跑在目标浏览器上，如何告知babel我们的目标浏览器呢?
- browserlist
- target

#### 命令行中使用
`browserlist`常见查询条件
`npx browerlist "> 1%, last 2 version, not dead"`
- Firefox > 20
- not ie <= 8
browserlist条件查询用到`caniuse-lite`工具

#### 配置文件中使用

```json
  "browserlist":["> 0.5%", "last 2 version", "not dead"]

```

```txt title=".browserlistrc"
> 1%
last 2 version
not dead
```


## react的jsx支持


## typescript的支持

创建typescript编译配置文件tsconfig.json
```sh
tsc --init
```
- [x] hybrid approach: babel for transpelling, tsc for types checking

[@typescript > handbook > project configuration > tsc cli options](https://www.typescriptlang.org/docs/handbook/compiler-options.html#handbook-content)
```sh
"sripts":{
    "type-check": "tsc --noEmit --watch"
}
```
