
- vue实现
- react实现

## 权限(vue3实现)

## 绘制登录页

- 添加登录页路由配置
`src/router/index.ts`

```diff
const routes: RouteRecordRaw[] = [
+  {
+  path: "/login",
+  name: "/login",
+  component: () => import("@/views/Login/index.vue")
+  }
]
```

- 创建登录表单
`views/Login/index.vue`
```vue
<template>
    <el-form class="login-form">
        <el-form-item prop="username">
            <el-input placeholder="用户名" v-model="loginForm.usename"/>
        </el-form-item>
        <el-form-item prop="password">
            <el-input placeholder="密码" v-model="loginForm.password"/>
        </el-form-item>
        <el-button type="primary" @click="handleClick">Login</el-button>
    </el-form>
</template>
<script lang="ts" setup>
import {reactive, toRefs} from "vue"
const loginState = reactive({
  loginForm: { username: "", password: ""}
})
const {loginForm} = toRefs(loginState)
const handleLogin = () => {
  loginForm
}
</script>

```


```


## 接入和调通登录接口

- 添加代理配置，使得`/api`路径的请求被代理到指定的后端服务器
`vite.config.ts`

```diff
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import path from "path";
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: [
      {
        find: "@",
        replacement: path.resolve(__dirname, "src"),
      },
    ],
  },
+ server: {
+   proxy: {
+     "/api": {
+       target: "http://localhost:3000",
+       ws: true,
+       changeOrigin: true
+     }
+   }
+ }

})
```

- 设置请求拦截
创建Axios实例并配置请求和响应拦截，ba
请求拦截：用于在请求头中添加token
响应拦截: 处理响应数据和错误
`src/api/request.ts`

```ts
import axios from 'axios'
import {ElMessage} from 'element-plus'
import {getToken} from '@/utils/auth'
const request = axios.create({
  timeout: 10000,
  baseURL: '/api'
})

request.interceptors.request.use(
  (config) => {
    const token = getToken()
    // 如果保存token则当前请求的请求头上携带token
    if (token){
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (err) => {
    console.log(`Request intercept error: ${err}`)
    return Promise.reject(err)
  }
)

request.interceptors.response.use(
  (response) => {
    if (response.data) {
      const {code,message} = response.data
      if (code !== 0) {
        ElMessage.error(message);
        return Promise.reject(new Error(message))
      }
      return response.data
    }
  },
  (err) => {
    console.error(`Response error: ${err}`)
    ElMessage.error(err.response?.data?.message || err.message || "发生错误")
    return Promise.reject(err)
  }
export default request
```

- 定义登录API请求
`src/api/user.ts` 存放用户相关API请求
```ts
import request from './request'
export const login = (data) => {
  return request.post("/auth/login",data)
}
```

- 认证工具方法:
定义了处理token的方法,包括获取、设置、移除token的方法
`src/utils/auth.ts`
```ts
const TOKEN = "token"
export const setToken = (token:stirng): void => {
  return localStorage.setItem(TOKEN, token)
}
export const getToken = (token:string): void => {
  return localStorage.getToken(TOKEN)
}
```

- 更新用户Store
`src/stores/user.ts`

```ts
import {defineStore} from 'pinia'
import {login} from '@/api/user'

export const userStore = definStore("user",() => {

  const state = reactive({
    token:''
  })

  const toLogin = async (loginInfo) => {
    const response = await login(loginInfo)
    const {token} = response.data
    state.token = token
    setToken(token) // 把token存在本地在每次发起请求时带上token
  }

  return {
    state,
    toLogin
  }
})

```



- 登录页

`src/views/Login/index.vue`

```vue
<template>
  <el-form className="login-form">
    <el-form-tiem prop="username">
        <el-input placeholder="用户名" v-model="loginForm.username"/>
    </el-form-tiem>
    <el-form-tiem prop="password">
        <el-input placeholder="密码" v-model="loginForm.password"/>
    </el-form-tiem>
    <el-button type="primary" @click="handleLogin">Login</el-button>
  </el-form>
</tempalte>

<script lang="ts" setup>
import {reactive, toRefs} from 'vue'
import {useUserStore} from '@/stores/user'
import {useRouter} from 'vue-router'
const userStore = useUserStore()
const router = useRouter()


const loginState = reactive({
    loginForm: { username:'',password:''}
})

const {loginForm} = toRefs(loginState)
const handleLogin = async () => {
  await userStore.toLogin(loginForm)
  router.push({path:'/'})
}

</script>

```

## 登录验证

1. 创建路由守卫，

这个守卫首先检查用户是否有效token，
如果持有有效token，并且尝试访问登录页面，他们会被重定向到登录页面，他们会被重定向到登录页面。
如果未登录，并且尝试访问非白名单路径，将会被重定向到登录页面，并附带原本尝试访问的路径信息

`src/permission.ts`

```ts
import router from '@/router'
const LOGIN_PATH = '/login'
const WHITE_LIST = [LOGIN_PATH]

router.beforeEach( to => {
  // 从本地中取出token
  const token = getToken()
  // 检查到token是有效的
  if (token) {
    if (to.path === LOGIN_PATH) {
      return { path: '/home', replace: true }
    }
    return true
  }
  if (WHITE_LIST.includes(to.path)) {
    return true
  }
  return {
    path: LOGIN_PATH,
    query: {redirce: to.path, ...to.path}
  }

})

```



- 在应用入口引入守卫

`src/main.ts`

```ts
import {createApp} from 'vue'
import App from './App.vue' //引入根组件
import router from './router/index'
import {createPinia} from 'pinia'
import './permission' // 路由守卫
const app = createApp(App)
app.use(router)
app.use(createPinia())
app.mount('#app')
```


## 登录成功后跳转

- 创建解析路由参数的钩子`useRouteQuery`
使用Vue Router 的`useRoute`来取得当前路由的查询参数， 并提取重定向地址`redirect`和其他查询参数`otherQuery`
- 重构登录组件
在登录页中，引入`useRouteQuery`钩子，在用户登录成功后，根据获取的`redirect`值来决定跳转地址。如果存在重定向地址，则跳转到该地址（并确保携带上剩下的路由参数）,否则则默认跳转到主页


`src/hooks/useRouteQuery.ts`

```ts
import {useRoute} from 'vue-router'
import {ref,computed, watchEffect} from 'vue'
const useRouteQuery = () => {
  const route = useRoute()
  const redirect = ref("")
  const otherQuery = ref({})

  const redirectComputed = computed(() => route.query.redirect || '')
  const getOtherQuery = (query) => {
    const {redirect, ...otherQuery} = query
    return otherQuery
  }
  const otherQueryComputed = computed(() => getOtherQuery(route.query))
  watchEffect(() => {
    otherQuery.value = otherQueryComputed.value
    redirect.value = redirectComputed.value as string
  })
  return {
    redirect,
    otherQuery
  }
}
```





## React实现

- 字段权限（密码）
- 功能权限
- 客户端路由权限
- 接口调用权限
请求
```ts

headers: {
  "Content-Type": "application/json",
  // "application/octet-stream" 文件
  //  "multipart/form-data"
  // "application/x-www-form-urlencoded" boydy of HTTP message sent to the server essentially query string: name/value pairs separted by ampersand(&).and names are separated from values by the equals symbol (=).
  "Accept": "application/json"
}

const WHITE_LIST = ['/login']
  requestInterceptors: [
    (url, config) => {
      const {apis} = currentUser
      // 接口调用权限检查
      const hasPermisssion = apis.some(api => api.method = config.method && new Regex(api.path).test(url)
        if (hasPermission) {
          return {url, config}
        } else {
          return
        }
    }
  ]
const {access_token, refresh_token} = tokens
```
