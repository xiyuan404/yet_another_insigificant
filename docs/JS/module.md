## a backgound on modules

split into separate module that can be imported as we need
One reason why ES6 introduced its own module format is to enable a static structure,

Benefit: dead code elimination during bundling

In frontend development, modules are usually handled as follows:

    During development, code exists as many, often small, modules.
    For deployment, these modules are bundled into a few, relatively large, files.

The reasons for bundling are:

    Fewer files need to be retrieved in order to load all modules.
    Compressing the bundled file is slightly more efficient than compressing separate files.
    During bundling, unused exports can be removed, potentially resulting in significant space savings.


## cjs和esm混合开发

### esm only

__filename, __dirname无法使用
```json
{
  "type": "module"
}
```
```js
import path from "node:path"
import {fileURLToPath} from 'node:url'

const __filename = fileURLToPath(import.meta.url)
const __dirname = __filename.slice(0,__filename.lastIndexOf("/"))
const pkgPath = path.resolve(__dirname, "./package.json")
```


### cjs only


```json
{
  "type": "commonjs"
}
```


##  import and export statements;

sideEffect export: prototype polyfill
## static module structure

conditionally loaded modules

## module format
modules:`"amd" | "umd" | "systemjs" | "commonjs" | "cjs" `

Load modules synchronously, while the body is executed - commonjs
Load all modules asynchronously, before the body is executed - amd
The synchronous syntax of Node.js plus the asynchronous loading of AMD -esm

## module loading: step,path

from entry point file to full depends graph
construction

1. find and fetch
   _module resolution_: where to download the file containing the module
   entry point `<script src="main.js" type="module"/>
   module specifier ` import loadh from "lodash"`
   importmap
   _fetch_:
   module cache
   the module file will only be fetched once even if multiple modules depend on it.
   module map
   using module cache to manage cache
   ![alt text](<ES modules A cartoon deep-dive - Mozilla Hacks - the Web developer blog-2.png>)
   What happens if another module depends on the same file? The loader will look up each URL in the module map. If it sees fetching in there, it will just move on to the next URL.
2. parse
   module record
   prase the file into module record
   ![alt text](<ES modules A cartoon deep-dive - Mozilla Hacks - the Web developer blog-3.png>)
   once the module recode is created, it's placed into module map
   ![alt text](<ES modules A cartoon deep-dive - Mozilla Hacks - the Web developer blog-4.png>)

If the main thread were to wait for each of these files to download, a lot of other tasks would pile up in its queue.
That’s because when you’re working in a browser, the downloading part takes a long time.

IN CommonJS can do things differently because loading files from the filesystem takes much less time than downloading across the Internet
![alt text](<ES modules A cartoon deep-dive - Mozilla Hacks - the Web developer blog.png>)
![alt text](<(1) X 上的 Srigi：“Latency Numbers Every Programmer Should Know It is hard for humans to get the picture until you translate it to human numbers httpst.coZeBMlgQOYq”  X.png>)
![alt text](<ES modules A cartoon deep-dive - Mozilla Hacks - the Web developer blog-1.png>)

##  export after declaration


inline vs clause

```js
//------ lib.js ------

function times(a, b) {
  return a * b
}
function sq(x) {
  return times(x, x)
}

const PI = 3.1415926

export { sq as square, PI }

//------ main.js ------
```

# default export

```js
//------ helper.js ------

function

export {
  as _
}

```

## module specifier resolution

control the resolution of module specifier

```js
<script type="importmap">
  {
    "imports":{
      "lodash": "/node_modules/lodash-es/lodash.js"//bare specifier
      "moment/": "/node_modules/moment/src/" //trailing slashes
      {/* in the Node.js ecosystem to import files without including the extension. trying multiple file extensions until we find a good match.*/}
      "lodash/fp":"/node_modules/lodash-es/fp.js"//Extension-less imports with import map
    "https://www.unpkg.com/vue/dist/vue.runtime.esm.js": "/node_modules/vue/dist/vue.runtime.esm.js" //url like specifier remapping
    "https://www.unpkg.com/vue/": "/node_modules/vue/" //prefix-like mapping

    }
  }
</script>

import _ from 'lodash'
{/* the above would act as if*/}
import _ from  "/node_modules/lodash-es/lodash.js"
```

## browser import map feature support detection
[feature support detection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLScriptElement/supports_static)
```js
if(HTMLScriptElement?.supports('importmap'){

}
```



## Mapping away hashes in script filenames

and with import statements that are of the form import "./sub-dep.mjs" instead of import "./sub-dep-7be2aa47f.mjs". Now, if we change sub-dep.mjs, we simply update our import map:

```js
{
  "imports": {
    "/js/app.mjs": "/js/app-8e0d62a03.mjs",
    "/js/dep.mjs": "/js/dep-16f9d819a.mjs",
    "/js/sub-dep.mjs": "/js/sub-dep-5f47101dc.mjs"
  }
}
```

## dynamic generate import map

```js
if (HTMLScriptElement?.supports('importmap')) {
  const im = document.createElement('script')
  im.type = 'importmap'
  im.textContent = JSON.stringfy({
    import: {
      lodash: '/node_modules/lodash-es/lodash.js',
    },
  })
}
document.currentScript.after(im)
```

// const script = document.currentScript
// scirpt.dateset.theme
// script.attributes.getNamedItem('src')

Resouce Link
[import map proposal](https://github.com/WICG/import-maps?tab=readme-ov-file#the-basic-idea)
[es module deep dive](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)

```

```

> Interaction with speculative parsing/fetching
>
> Most browsers have a speculative HTML parser which tries to discover resources declared in HTML markup while the HTML parser is waiting for blocking scripts to be fetched and executed.


## import.meat






[^1]: [JavaScript Module](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
[^2]: [@exploringjs > modules ](https://exploringjs.com/es6/ch_modules.html#static-module-structure)
