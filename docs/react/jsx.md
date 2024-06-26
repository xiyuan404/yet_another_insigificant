

<h2  style="font-size: 22px; display: inline-block; padding-left: 10px; border-left: 5px solid rgb(145, 109, 213); font-weight:bold;">JSX expression</h2>

`JSX expression`, ues to create `ReactElement`,

which wil be compile to  calling the API, `React.creatElement(type, props, ...children)`

we can use `JSX expression` anywhere we could use any other expression( `name` 、`call expression` 、`literal` 、`member experssion`)



<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">props</span></h3>

Any JSX attributes become props(parameters) of the ReactElement. The value of an attribute can be a string, like `status="pending"`,  or it can be any Javascript expression when wrapped in curly braces, as in `onLoad={loadingAsset}`

(which will set the value of the `onLoad` prop to `loadingAsset`)

<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">children</span></h3>

Any children elements should go between the opening tag `<View>`,  and closing tag `</View>`, element  without a children can use a self-closing tag, like`<View/>`,  as a shorthand

<h3 style="text-align:center;  font-size: 20px;font-weight: bold;"><span style="border-bottom: 2px solid rgb(145, 109,213);">interpolation</span></h3>

children can generally other elements, but can also be any other kind of expression if wrapped in curly braces `{...}`

```jsx
const hasbeenLoaded = useState(false)
const element = (
	<View>
  	<AppLoading/>
    {hasbeenLoaded}
  </View>
)
```

