 node服务器，处理浏览器各种资源的加载

 - 宿主页
 - js文件加载
 - 裸模块文件路径重写和加载
 - 解析SFC
 - 代码生成



 ```html
 <body>
     <script>
     window.process = {
        NODE_ENV = 'dev'
     }
     </script>
 </body>
 ```


 ```js
 const Koa = require("koa")
 const app = new Koa()
 const fs = require("fs")
 const paht = require("path")

 app.use(ctx => {
   const {url} = ctx.request
   if (url === '/'){
    ctx.type = "text/html"
    ctx.body = readFileSync('./index.html', 'utf-8')
   } else if (url.endsWith('.js')) {
     const p = path.join(__dirname, url)
     ctx.type = "application/javascript"
     ctx.body = rewriteImport(refs.readFileSync(p,'utf-8'))
   } else if (url.startsWith('/@modules/'){
     // 取得模块名称
     const moduleName = url.replace('/@modules/', '')
     // 去node_modules目录中找
     const prefix = path.join(__dirname, "../node_modules", moduleName)
     // package.json中取得module字段
     const moduleField = require(prefix + "package.json").module
     const filePath = path.join(prefix, module)
     const fileContent = fs.readFileSync(filePath, "utf-8")
     ctx.type = "application/javascript"
     ctx.body = rewriteImport(fileContent)
   } else if (url.indexOf('.vue' > -1) {
     const p = path.join(__dirname, url)
     const fileContent = fs.readFileSync(p, 'utf-8')
     const fileParsed = compulerSFC.parse(fileContent)
     ctx.type = "application/javascript"
     ctx.body = fileParsed
   }
 })

 function rewriteImport(content) {
   return content.replace(" from ['"](.*)['"]", (match, p) => {
     if(p.startsWith('./') || p.startsWith('../') ||p.startsWith('/')) {
       return match
     } else {
        return ` from '/@modules/${p}'`
     }
     })
 }


 app.listen(3000, ()=> {
   console.log("kvite startup!!!")
 })

 ```
