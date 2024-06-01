



## resize
```js

document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
window.addEventListener('resize',() => {
  document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
})

```
![alt text](images/*height.png)
- offsetHeight 

Height occupied by the element on document

- clientHeight:
only `VISIBLE content & padding`
CSS height + CSS padding - height of horizontal scrollbar


- scollHeihgt
`ENTIRE  content & padding (visible or not)`
Height of all content + paddings, despite of height of the element.

- Element.getBoundingClientRect()