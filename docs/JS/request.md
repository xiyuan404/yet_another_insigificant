# 手写请求

- [x] ajax
- [ ] fetch
- [ ] axios





```js
const ajax = (option) => {
  const objToString = (data) => {
    data.t = new Date.getTime()
    const res = []
    Object.fromEntries(data).forEach(([key, value]) => {
      res.push(encodeURIComponent(key) + '=' + encodeURIComponent(value))
    })
    return res.join('&')
  }

  objToString(option.data || {})
}
```

