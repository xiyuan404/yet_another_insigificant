# react-router-dom useNavigate

??? bug "relative current route when navigate"
    Product:	react-router-dom ▾

    Component:	useNavigate Hook ▾

    Version:	"react-router-dom": "^6.23.1",
    
    Platform:	Desktop	Macos

当前搜索页的url
`http://localhost:3000/searchList/201235/鸡肉`

触发页面跳转的 tag
```jsx
<li
  className="list-item"
  key={item + index}
  onClick={() => handleKeywordClick(item)}
>
  {item}
</li>
```
箭头函数跳转结果
`http://localhost:3000/search/201235/searchList/201235/鸡肉`
```js
const handleKeywordClick(keyword) => {
    navigate(`/searchList/${params.shopId}/${keyword}`);
  }

```

函数声明跳转结果
`http://localhost:3000/searchList/201235/鸡肉`
```js
function handleKeywordClick(keyword) {
  navigate(`/searchList/${params.shopId}/${keyword}`)
}
```

