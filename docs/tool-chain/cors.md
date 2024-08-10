## 源
[protocol,port,host]元组

## 同源策略
如果两个url的protocol、port 和 host都相同，则它们是同源的

##  跨域
前后端分离的架构中静态资源服务器和API接口服务器往往不是同一个服务器或不是同一个接口

## resouce loading

The browser allows us to track the loading of external resources – scripts, iframes, pictures and so on.

There are two events for it:

onload – successful load,
onerror – an error occurred.

When the browser loads HTML and comes across a <script>...</script> tag, it can’t continue building the DOM. It must execute the script right now. The same happens for external scripts <script src="..."></script>: the browser must wait for the script to download, execute the downloaded script, and only then can it process the rest of the page.

- [x] that leads to two issues:
1.scirpt can't see DOM elements(DOM parse pause), so they can add handlers
2.If there’s a bulky script at the top of the page, it “blocks the page”. Users can’t see the page content till it downloads and runs:
`<script async>`
`<script defer>`



No `crossorigin` attribute – access prohibited.

`crossorigin="anonymous"` – access allowed if the server responds with the header Access-Control-Allow-Origin with * or our origin. Browser does not send authorization information and cookies to remote server.

`crossorigin="use-credentials"` – access allowed if the server sends back the header Access-Control-Allow-Origin with our origin and Access-Control-Allow-Credentials: true. Browser sends authorization information and cookies to remote server.





```js
<script crossorigin="use-credentials"/>
```


## 跨域解决方案
将静态资源部署在和API接口所在服务器，就不会产生跨域问题
cors
服务器标识除自己以外还有哪些源外可以访问服务器中的资源






```js
app.use((ctx,next)=>{
  ctx.set('Access-Control-Allow-Origin', 'http://localhost:*')
  ctx.set('Access-Control-Allow-Crendentials', true)
  ctx.set('Access-Control-Allow-Headers', 'Connection')
  ctx.set('Access-Control-Allow-Methods', 'Get,Post')
})
```
node代理服务器(webpack-dev-server配置原理)
```js
const {createProxyMiddleware} = require('http-proxy-middleware')
// 将静态资源和代理在同一服务器
app.use(express.static('./client'))  // working directory as root

app.use('/api', createProxyMiddleware({
  target:"http://localhost:8000",
  pathRewrite:{
    '^/api': ''
  }
}))

app.listen(9000,()=>{console.log('server start on port 9000')}
```


```js title="webpack.config.js"
devServer: {
    proxy: [
      {
        context: ['/api'],
        target: 'http://localhost:3000',
        changeOrigin: true, // 验证请求来源
      },
    ]
}
```

- [x] nginx反向代理


```nginx title="nginx.conf"
http {
  server {
    listen 80;
    location / {
      add-header Access-Control-Allow-Origin *;
      add-header Access-Control-Allow-Crendential true;
      if ($request_method = "OPTIONS") {
        return 204; # no content
      }
      proxy_pass http://localhost:8000
    }
  }
}



```

Not practical in project dev
jsonp
websocket
postMessage
