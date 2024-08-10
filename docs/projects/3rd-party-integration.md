## 创建项目

```js
pnpm create vite vue3-auth --template vue-ts
cd vu3-auth
```
## 集成vue路由

1. 安装依赖
```bash
pnpm install vue-router
```

2. 引入路由
```diff
src\main.ts
import { createApp } from 'vue'
+import router from "./router/index"
import App from './App.vue'
const app = createApp(App)
+app.use(router)
app.mount('#app')
```

3. 配置路由
`src\router\index.ts`
```ts
import { createRouter, createWebHistory, RouteRecordRaw } from "vue-router";
const routes: Array<RouteRecordRaw> = [
    {
      path: "/",
      redirect: "/home",
    },
    {
      path: "/home",
      component: () => import("../views/Home/index.vue")
    },
  ];
export default createRouter({
  history: createWebHistory(),
  routes,
});
```




## 配置vite路径别名

`vite.config.ts`
```diff
export default defineConfig({
  plugins:[vue()],
+  resolve:{
+    alias: [
+      {
+        find: "@",
+        replacement: path.resolve(__dirname, "src")
+      }
+    ]
+  }
})
```

`tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",

    /*  Linting */
    "strict": false,
    "noImplicitAny": false,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "types": ["element-plu/global"]
  },
  "include":["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{"path":"./tsconfig.node.json"}]
}
```


## 集成Pinia

- 安装依赖：`pnpm install pinia`
- 引入pinia
`src/main.ts`
```ts
import {createApp} from 'vue'
import App from './App.vue'
import router from './router/index'
+import {createPinia} from 'pinia'

const app = createApp(App)
app.use(router)
+app.use(createPinia())
app.mount('#app')

```

`src/stores/user.ts`
```ts
import {defineStore} from 'pinia'
import {reactive} from 'vue'


export const useUserStore = defineStore("user", () => {
  const state = reactive({
    token: ""
  })
  const toLogin = async(loginInfo) => {
    const response = await login()
    const {token} = response.data
    state.token = token
    setToken(token)
  } catch (err) {
    cosnole.log(`error: ${error}`)
    return Promise.reject(err)
  }
  return {
    state,
    toLogin
  }
})
```

`src/vies/Login/index.vue`
```ts
<script lang="ts" setup>
import {useUserStore} from '@/store/user'
import {useRouter} from 'vue-router'

const userStore = useUserStore()
const router = useRouter()


const handleLogin = () => {
  useStroe.toLogin(loginState.loginForm)
  router.push({path:"/"})
}

```




## 集成element-plus

`tsconfig.json`

修改ts配置,访问组件类型定义文件
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "module": "ESNext",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "preserve",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
+   "types": ["element-plus/global"] // 组件类型文件
  },
  "include": ["src/**/*.ts", "src/**/*.tsx", "src/**/*.vue"],
  "references": [{ "path": "./tsconfig.node.json" }]
}

全局引入

```ts
import ElementPlus from 'element-plus'
import "element-plus/dist/index.css"
app.use(ElementPlus)

```

按需引入

```
