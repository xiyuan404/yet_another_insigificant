<head>  <style>    :root {      --color-primary: rgb(145,109,213)    }    code {      color: var(--color-primary) !important;    }  </style> </head>


- [ ] - [x] 绘图
- [ ] 动画
- [ ] 地理GIS
- [ ] drag and drop
- [ ] 音视频流媒体
- [ ] 安全
- [ ] 性能
- [ ] 网络

- [x] web storage( localStroage and sessionStorage)
- [ ] video timed Track / Aduio data API
- [ ] IndexDB and web SQL
- [ ] web Sockets
- [ ] Geo-location
- [ ] Drag and Drop
- [ ] WebGl
- [ ] HTML5 frame Fragment Web Component
- [ ] Web API



<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">Javascript engines</h2>

Our Javascript codes run inside a Javascript engine, on browser, our code run in `V8` or `webkit`,  on IOS,. our code runs in `JSC`,

on Android, our code can either run in `JSC` or a new engine from Facebook called `Hermes`

Each Javascript engine, and each version of Javascript engine, suppors slightly different  Javascript language features,  The `JSC` used by IOS 12 may not support the __exact same__ feature set as the one in IOS 13 or IOS 14, and it will __almost certainly__ be a little different than `Herms`

We don't want to have to think about this complexity when writing an app - thats where `Babel` comes in.

<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">Babel</h2>

Babel is a highly configurable compiler that les us use newer Javascript language features(and extensions, like JSX), compiling down into older versions of Javascript that are support on a wide range of engines. This essentially smooths out any different `JSC` version, or between `JSC` and `Herms`, so we don't have to think about those differences when writing our app.



__fetatue dection and polyfill__

```ts
if(CSS.supports("subgrid")){}
```



<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">Typescript</h2>

JavaScript is dynamically typed, which means the type checking  

at runtime. This is convenient for small-scale app. but it's often helpful to have type-checking at compile time. For this reason, many .

Typescript extends JavaScript, adding syntax for type declarations and annotation. The Typescript compiler can perform compiler-time type checking. Before transforming the Typescript code into Javascript.

<h2  style="font-size: 18px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">Up next</h2>

Let's go thorught some a few Javascript language features, and then jump into Javascript language extesion, before moving into React
