



## resize
```js

document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
window.addEventListener('resize',() => {
  document.documentElement.style.fontSize = document.documentElement.clientWidth / 375 * 100 + 'px'
})

```
## white-space bahaviour

css white space property

|            | line breaks char | extra while space | Text wrapping |
| :--------- | :--------------- | :-------------- | :------------ |
| `normal`   | Collapse         | Collapse        | Wrap          |
| `pre`      | Preserve         | Preserve        | No wrap       |
| `nowrap`   | Collapse         | Collapse        | No wrap       |
| `pre-wrap` | Preserve         | Preserve        | Wrap          |
| `pre-line` | Preserve         | Collapse        | Wrap          |


## all about height

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