
`index.spec.js`

```js

reverseReange(workArr, start, start + runLength)

```

```js

const reverseReange = (arr, from, to) => {
  while (from < to) {
    const startElem = arr[from]
    const endElem = arr[to]
    arr[from++] = endElem
    arr[to--] = startElem
  }
}



```
