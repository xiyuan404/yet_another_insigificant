# 前后端数据通信

浏览器:提供web服务的客户端浏览程序,可向web服务发送各种请求,并对从服务器发来的超文本和各种多媒体数据格式进行解析,显示和播放

服务器: 指一个管理资源并未用户提供服务的计算机软件, 通常为文件服务器,数据库服务器,应用程序服务器. 运行以上软件的计算机或计算机系统也被称为服务器





cors  反向代理





http response status code(响应状态码)

400 Bad Request 语法错误

401 Unauthorized 未认证

403 Forbidden 禁止访问

404 Not Found 资源未找到

500 Interal Server Error 服务器故障

503 Service Unavailiable 服务器繁忙



## 局部刷新与异步

瀑布流内容下拉刷新列表,上拉加载更多

地图拖拽加载更多



## GET发送数据





## POST发送数据

__constructing request body type__

![request body type](images/request-body-type.png)



`Content-Type: multipart/form-data`

> The content type "application/x-www-form-urlencoded" is inefficient for sending large quantities of binary data or text containing non-ASCII characters.
>
> "multipart/form-data" should be used for submitting forms that contain files, non-ASCII data, and binary data.

`Content-Type: application/x-www-form-urlencoded`

```js

xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')//Raw text/plain
xhr.send('username=James&password=12345')
```

`raw json`

`Content-Type: application/json`

```js
xhr.setRequestHeader('Content-type', 'application/json')
xhr.send(JSON.stringfy({}))
```

![mime](<images/MIME types (IANA media types) - HTTP  MDN.png>)

__raw text/plain__

`Content-Type: text/plain`





JSON



## 响应数据类型

xhr.responseText

xhr.responseXML



```js
router.post('/login', (ctx, next)=>{
  const token = require('crypto').randomBytes(64).toString('hex')
  ctx.body = {
 		errcode: 0,
  	errmsg: 'ok',
  	result: {
     	   permissons:['LIST:UPDATE', 'LIST:DELTE'],
    			token 
    }
  }
 
})
```



## 把响应JSON数据渲染到页面

模版字符串支持插值表达式

```js
const boday = document.querySelector('tbody')
xhr.onload = function(){
	const data = JSON.parase(xhr.responseText)
  tbody.innerHtml = data.result.permissions.map(permission => {
    return `
    		<td>${permission}<td>
    `
  })
}

```

模版引擎



## API接口文档

swagger

错误码

- -1 : 参数个数错误
- -2: 参数类型错误

```js
const fs = require('fs')
router.post('/page', async(ctx, next)=>{
	const args = [
  	{ field: 'page', type: 'number'},
  	{ field: 'count', type: 'number'}
	]
	const requestBody = ctx.request.body // { page: 1, count: 10 }
	args.forEach(arg => {
  	if(!Object.keys(requestBody).includes(arg.field))	{
  		ctx.body = {
  			errcode: -1,
  			errmsg: '参数个数错误'
  		}
  		return; 
  	}else {
  		if(typeof requestBody[arg.field] != arg.type){
  			ctx.body = {
  				errcode: -2,
  				errmsg: '参数类型错误'
  			}
  			return;
  		}
  		
  		let data = fs.readFileSync('./data/sportNewsList.json')
  		data = JSON.parse(data)
      const result = data.splice((requestBody.page - 1) * requestBody.count, requestBody.count)
      ctx.body = {
        errcode: 0,
        errmsg: 'ok',
        result
      }
  	}
	})  
})

```

