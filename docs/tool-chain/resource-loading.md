[CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
[Speculation Rules API](https://developer.mozilla.org/en-US/docs/Web/API/Speculation_Rules_API)





## script loading and execution timing

![](assets/legend.svg)

- [x] <script> without any attributes
![](assets/script.svg)
parsed HTML Document to DOM meet the script tag,parsing will stop,
a request will be made to fetch the file (if it’s external).
The script will then be executed before parsing is resumed.
- [x] <script async>
parallel download the script file
pause the HTML parser to execute it when it has finished downloading.
only execute it after the parser has completed


![](assets/script-async.svg)
![](assets/script-defer.svg)




[<script>: type attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type)
[script](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script)



## rosource loading

The browser allows us to track the loading of external resources – scripts, iframes, pictures and so on.

There are two events for it:
- onload – successful load,
- onerror – an error occurred.


In modern websites, scripts are often “heavier” than HTML: their download size is larger, and processing time is also longer.

When the browser loads HTML and comes across a <script>...</script> tag, it can’t continue building the DOM. It must execute the script right now. The same happens for external scripts <script src="..."></script>: the browser must wait for the script to download, execute the downloaded script, and only then can it process the rest of the page.


That leads to two important issues:

1.Scripts can’t see DOM elements below them, so they can’t add handlers etc.

2.If there’s a bulky script at the top of the page, it “blocks the page”. Users can’t see the page content till it downloads and runs:



## <scirpt> type attribue



The value of this attribute indicates_ the type of data represented by the script, and will be one of the following:
import

- Attribute is not set (default), an empty string, or a JavaScript MIME type
  - text/javascript: is a legacy value and used as the default if you omit the type attribute in a script tag
  - application/javascript: is recommended for current browsers.

- importmap
- module
- speculationrules

### importmap

control how the browser resolves module specifiers when importing JavaScript modules.

- [x] bare module

- [x] Mapping path prefixes

>  in this case the property and mapped path must both have a trailing forward slash (/)


```html
<script type="importmap">
  {
    "imports": {
      "@/": path.join(__dirname, './src/')
    },
    "scopes": {
    }
  }
</script>
`


```js
// classic, module, importmap, speculationrules
if(HTMLScriptElement.supports("importmap")){

}
```


[^1]: [script tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type)
[^2]: [HTMLScriptElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLScriptElement)
