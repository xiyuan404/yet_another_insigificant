● 从零搭建 vue3 开发环境, 详解 vue 设计理念和设计架构
● 手写 vue3 响应式原理, reactive, effect, watch, computed, ref 原理
● 掌握 vue3 源码调试技巧,掌握响应式数组,map,set 处理
● 手写自定义渲染器原理,以及 runtimeDOM 中属性,事件处理
● 手写虚拟 dom 原理,手写 vue3 中 diff 算法及最长递增子序列实现原理
● 手写组件渲染原理,组件挂载流程,及异步渲染原理
● 手写 vue3 中生命周期原理,props,emits, slot, provide, inject 实现原理
● 详解 vue3 编译优化,patchFlags,blockTree, 实现靶向更新
● 模版转化 ast 语法树, 掌握语法树优化及代码生成原理
● 手写 vue3 中异步组件原理,函数式组件,teleport,keep-alive,transition 组件
● 手写 pinia 以及 vueRouter 实现原理
● vu3 中单元测试和服务端渲染

1.声明式编程

```js "chainDataProcess"
const chainDataProcess = (str) =>
  str
    .toLowerCase()
    .split('')
    .map((c) => c.trim())
    .reverse()
    .filter((x) => x.length > 3)
    .join('')
```

```js "impreativeDataProcess"
const imperativeDataProcess = (str) => {
  const lower = str.toLowerCase()
  const words = lower.split('')
  for (let i in words) {
    words[i] = words[i].trim()
  }
  words.reverse()
  let keepers = []
  for (let i in words) {
    if (words[i].length > 3) {
      keepers.push(words[i])
    }
  }
  return keepers.join('')
}
```

```js "composeDataProce"
function compose(...fns){
  if(fns.length === 0) return input => input
  if(fns.length === 1) return input => head(fns)input
}
  return function(input){
    return compose(...tail(fns))(head(fns)(input))
  }
function head(arr){
  return arr[0]
}

function tail(arr){
  return arr.slice(1)
}
function desnake(snake_case_string){
  return snake_case_string.split('_')
}
function capFirst(str){
  if(str.length < 1) return
  return str.charAt(0).toUpperCase + str.substring(1).toLowerCase()
}

function capAll(strArr){
  return strArr.map(str => capFirst(str))
}
function camelize(strArr){
    return [head(strArr)].concat(capAll(tail(strArr))).join('')
}

function hyphenate(strArr){
  return [head(strArr)].concate(tail(strArr).join('-'))
}

const snakeToCamel = compose(desnake, camlize)
const snakeToKabab = compose(desnake, hyphenate)
```

```js "curry"
function I18n(greeting) {
  return function (name) {
    return [greeting, name + '!'].join(' ')
  }
}
const greetItal = I18n('Ciao')
const greetFren = I18n('Bonjour')
const greetSpan = I18n('Hola')

/*========== Test Case ==========*/
greetItal('Hammond') // Ciao Hammond!
greetFren('Hammond') // Bonjour Hammond!
greetSpan('Hammond') // Hola Hammond!
```

### 2.采用虚拟 DOM

```js
const ulEl = document.querySelector('tbody')
xhr.onload = function () {
  const data = JSON.parase(xhr.responseText)
  ulEl.innerHtml = data.result.permissions.map((permission) => {
    return `
    		<l>${permission}<li>
    `
  })
}
```

## Monroepo 管理项目

**vue 分包管理**

vue 同一个 repo(仓库)package(分包)管理

```bash
cd core
tree -d packages/ -L 1
```

```"core/packages/"
packages/
├── compiler-core #平台无关编译核心
├── compiler-dom # 针对浏览器的编译模块
├── compiler-sfc # 针对单文件解析
├── compiler-ssr # 针对服务端渲染的编译模块
├── dts-built-test #
├── dts-test
├── reactivity # 响应式系统
├── runtime-core
├── runtime-dom
├── runtime-test
├── server-renderer #用于服务端渲染
├── sfc-playground
├── shared # 多包共享
├── template-explorer # 调试编译器输出的开发工具
├── vue # 完整版本,包括运行时和编译时
└── vue-compat # 迁移构建,用于兼容vue2
```

**打包产物**

```"package.json"
{
	"scirpts": {
		"dev": "node scripts/dev.js reactivity -f esm"
	}
}
```

**打包格式**

```bash
tree  node_modules/@vue/reactivity/dist -L 1

node_modules/@vue/reactivity/dist
├── reactivity.cjs.js
├── reactivity.esm-browser.js
├── reactivity.esm-bundler.js
├── reactivity.global.js
├── reactivity.cjs.prod.js
├── reactivity.esm-browser.prod.js
├── reactivity.d.ts
└── reactivity.global.prod.js

```

### 1. 全局安装 pnpm

```bash
npm install pnpm -g
pnpm -init
```

```".npmrc"
shamefully-hoist = true # vue 依赖的包提升到 node_moduels root
import {} from '@vue/reactivity' // 幽灵依赖
```

```".npmrc"
shamefully-hoist = false # default

`pnpm install @vue/reactivity -w`
import {} from '@vue/reactivity'
```

```"pacakage.json"
{
	name: "monorepo-test"
	private: true,
}
```

```yaml "pnpm-wrokspace.yaml"
// root/main/base project
//	- packages:
//		- subProject1
//		- subProject2
packages:
	- "packages/*"
```

add vue dependency to workspace root

```bash
pnpm install vue --workspace-root
```

项目分包入口

**typescript 配置**

```yaml
- packages
 -reactivity
 	- src
 		-index.ts

`pnpm install typescript -w`
pnpm tsc --init
```

```json "tsconf.json"
{
  "compilerOptions": {
    "outDir": "dist",
    "sourceMap": true,
    "target": "es2016",
    "module": "esnext",
    "moduleResolution": "node",
    "strict": false,
    "resolveJsonModule": true,
    "esModuleInterop": true,
    "jsx": "preserve",
    "lib": ["esnext", "dom"],
    "baseUrl": ".", // 配置文件所在目录
    "paths": {
      "@vue/*": ["packages/*/src"] // modudle specifier 解析
    }
  }
}
```

```yaml
- packages
  - reactivity
  - src
  - index.ts
  - shared
  - src
  - index.ts
```

作用域包避免包发布冲突

```json "package.json"
{
  "name": "@vue/ractivity"
}
```

```json "package.json"
{
  "name": "@vue/shaed"
}
```

**引入分包**

```bash
pnpm install @vue/shared@workspace --filter @vue/reactivity
```

```"package.json"
"dependencies": {
    "@vue/shared": "workspace:^"
 }
```

```json "tsconfig.json"
{
  "baseUrl": ".", //在当前配置文件作在目录
  "paths": {
    "@vue/*": ["packages/*/src"]
  }
}
```

## 开发环境搭建

## 手写 effect 和 reactive

```json
{
  "id": 1,
  "subject": "hands on vue ",
  "dueDate": {
    "DateTime": "2016-04-25T07:00:00.0000000",
    "TimeZone": "UTC"
  },
  "priority": "High",
  "completed": false 
}
```



```ts title="reactive.ts" linenums="1"
import { isObject } from '@vue/shared'

import { ReactiveFlags, mutableHandlers } from './baseHandler'

export function reactive(target) {
  return createReactiveObject(target)
}

const reactiveMap = new WeakMap()

function createReactiveObject(target) {
  if (!isObject(target)) {
    return
  }
  if (target[ReactiveFlags.IS_REACTIVE]) {
    return target
  }
  // 防止同一个对象被代理两次, 返回的永远是同一个代理
  let existingProxy = reactiveMap.has(target)
  if (existingProxy) {
    return reactiveMap.get(target)
  }

  const porxy = new Proxy(target, mutableHandlers)
  reactiveMap.set(target, proxy)

  return proxy
}
```

```html title="index.html" linenums="1"
<body>
  <div class="app"></div>

  <script>
    /*========= Test Code ===========*/
    import { reactive, effect } from './reactivity.js'

    const todo = {title: 'hands on vue', completed: true, flag: }

    const state = reactive(todo)

    effect(()=>{
      //		state.age = Math.random()
        // flag = true, 收集的依赖为flag, name
        // flag = flase, 收集的依赖为flag, age
        app.innerHTML = stat.flag ?  state.title : state.completed
        effect(()=>{
		
        })
      }
    )
  </script>
</body>
```

```typescript
const mutableHandlers = {
  get(target, key) {
    track(target, key)
    let result = Reflect.get(target,key,receiver)
    if(isObject(result)){
      return reactive(result)
    }
    return result
  },
  set(target, key, value, revier) {
    console.log('设置新的值, 触发更新')
    let oldValue = target[key]
    if (value !== oldValue) {
      trigger(target, key, value, oldvalue)
    }
  },
}
```


```ts title="effect.ts"
export activeEffect = undefined

class ReactiveEffect {
	parent = undefined;
	constructor(fn, scheduler){}
  
  run(){
    try{
      this.parent = activeEffect
      activeEffect = this
      return this.fn()
    }finally {
      activeEffect = this.parent
      this.parent = undefined
    }
  }
}



export function effect(fn, options = {}){
  const _effect = new ReactiveEffect(fn, options.scheduler)
  _effect.run()
  const runner = _effect.run.bind(_effect)
  return runner 
}
```





```typescript
function trigger(target, key, value, oldvalue) {
  // 找到effect然后执行
  const depsMap = targetMap.get(target)
  if (!depsMap) {
    // 任何字段的effect都没有
    return
  }
  const effects = depsMap.get(key)

  if (effects) {
    effects.forEach((effect) => {
      // 当前正在执行的和现在要执行的是用一个就屏蔽掉
      if (activeEffect !== effect){
        if(effect.scheduler){
          effect.scheduler()
        }else {
          effect.run()
        }
      } 
    })
  }
}
```

**依赖收集原理**

![effect_collection](images/effect_collection.png)

> todo = {title: 'hands on vue', completed: 'false'}
>
> const targetMap = {
>
>  todo: {
>
>  title: [effect1, effect2],
> ​ completed: [effect1, effect3]
>
>  }

```typescript

function track(target, key){
  let depsMap = targetMap.get(target)
	if(!depsMap){
    targetMap.set(target,(depsMap = new WeakMap())
  }
	let effects = depsMap.get(key)
  if(!effects){
		depsMap.set(key, (effects = new Set()) )
  }
  let shouldTrack = !effects.has(effect)
  if(shouldTrack){
    effects.add(activeEffect)
    activeEffect.deps.push(effects)
  }
}
}
```

- [x] 异步批量更新

```html title="index.html"
/*======= Effect Test code =========*/
<body>
  <script>
    const runner = effect(
      () => {
        app.innerHtml = state.title
      },
      {
        scheduler() {
          if (!isFlushing) {
            Promise.resolve().then(() => {
              runner()
              isFLushing = false
            })
          }
          isFlushing = true
        },
      }
    )
  </script>
</body>

```

- [x] 条件收集, 清空上次依赖收集


```ts
effect(()=>{
  app.innerHTML = todo.id ? todo.priority : todo.subject 
})
```

- [x] 深度懒代理

```ts
effect(()=>{
  app.innerHTML = todo.dueDate.dateTime
```



## ref, toRefs, toRef, toRaw, proxyRefs

``` ts ref.ts


function proxyRefs(target){
  return new Proxy(target,{
    get(target, key, receiver){
       let r = Reflect.get(target.key,receiver){
       return  r.__v_isRef ? r.value : r
      },
	  set(target, key, value, receiver){
				const oldValue = target[key];
      	if(oldValue.__v_isRef){
          oldValue.value = value
          return
        } else {
          return Reflect.set(target, key, value, receiver)
        }
      	
     	}
    }
  })
}

```

