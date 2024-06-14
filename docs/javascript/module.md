# where to install import map

import keyword is only available inside a script tag with the type="module" attribute.

# before we have module we have import

how you manage/organzie you variable
dependencies are implict, which function depend on which script?

# module loading: step,path

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

export { sq as square, LS as LIGHTSPEED }

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

# import map

control the resolution of module specifier map

```js
// added to head section
gcc    -lpthread    -g     csapp.c    echo_client.c    -o    client
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

## import.meta

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