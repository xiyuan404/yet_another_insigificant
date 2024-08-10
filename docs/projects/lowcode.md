低代码开发平台： 采用图形化拖拽和配置参数的方式完成开发工作
组成： 组件区、编辑区、预览区、属性区、事件区，通过拖拽生成JSON,通过JSON渲染页面


编辑器布局

`src/data.json`

```json
{
  "container": {
    "width": 500,
    "height": 500
  },
  "blocks": [
    {"top":100, "left": 100, "zIndex": 1, "key": "text"},
    {"top":200, "left": 200, "zIndex": 1, "key": "button"},
    {"top":300, "left": 300, "zIndex": 1, "key": "input"},
  ]
}
  blocks

```

`src/App.vue`

```vue
<template>
    <Editor v-model={state}/>
</template>


<script >
import {ref, provide} from 'vue'
import data from './data,json'
import Editor from './packages/editor'
import {registerConfig as config} from './utils/editor-config'
export default {
  component: {
    Editor
  },
  setup() {
    const state = ref(data)
    provide("config",config) // 配置透传
    return {
      state
    }
  }
}
</script>

<style>
.app {
    /* 四周留白 */
    position: fixed;
    inset: 15px;
}

.line-v {
    position: absolute;
    top: 0;
    bottom: 0;
    border-left: 1px solid dashed;
}


.line-h {
    position: absolute;
    left: 0;
    right: 0;
    border-top: 1px solid dashed

}
</style>
```



`src/packages/editor.jsx`

```jsx
import {defineComponent, computed, inject} from 'vue'
export default defineComponent({
  props: {
    modelValue: {tyep:Object, required: true}
  },
  setup(props){
    const data = computed(() => {
      get() {
        return props.modelValue
      }
    })

    const containerStyle = {
      width: data.value.container.width + 'px',
      height: data.value.container.width + 'px'
    }


    const foucsData = computed(() => {
        const focus = []
        const unfocusd = []
        data.value.
      })


    const config = inject("config")

    return () => (
      <div class="editor">
        <div class="editor-left">
          cofig.componentList.map(component => {
            <div class="editor-left-item">
              <span>{component.label}</span>
              <div>{component.preview()}<div>
            </div>
          })
        </div>
        <div class="ediotr-top"> 顶部菜单栏</div>
        <div class="ediotr-right">右侧属性栏</div>
        <div class="editor-container">
            {/* 滚动条 */}
          <div class="editor-container-canvas">
            {/* 内容 */}
            <div class="editor-container-content" style={containerStyle}>
              {
                data.blocks.map(block => (
                  <Editor-block block={block}></Editor-block>
                ))
              }
            </div>
          </div>
        </div>
      </div>
    )

  }
})

```


`src/packages/editor.scss`

```ssss
.editor {
  width: 100%;
  height: 100%;
  position: relative
  &-left, &-right {
    poistion: absolute;
    top:0,
    bottom:0,
    width: 270px;
  }
  &-left {
    left: 0;
    &-item{
    width: 250px;
    display: flex;
    align-items:center;
    justify-content:center;
    background:#fff;
    padding:20px;
    cursor:move,
    user-select:none;
    margin-top: 20px;
    position: absolute;
    > span {
      position:absolute;
      top:0;
      left:0;
      background:
      padding:tea;
      padding: 4px;
    }
    &::after {
      position:absolute,
      content: "",
      inset:0
    }
    }

  }
  &-right {
    right: 0
  }
  &-top {
    position: absolute;
    left: 280px;
    right: 280px;
    height: 80px;
  }
  &-container {
    padding: 80px 270px 0;
    height: 100%;
    box-sizing: border-box;

    &-canvas {
      overflow: scroll;
      height: 100%;
      &__content {
        positon: relative;

      }
    }
  }
}

.editor-block {
  position: absolute;
}

```


`src/packages/editor-block.jsx`

```jsx
import {defineComponent, computed, inject} from 'vue'
export default defineComponent({
  props: {
    block: {tyep:Object}
  },
  setup(props) {


    const config =  inject("config")


    const blockStyle = computed(() => {
      top: `${props.block.top}px`,
      left: `${props.block.top}px`,
      zIndex: `${props.block.zIndex}`
    })

    return () => {
      const component = config.componentMap[props.block.key]
      const RenderComponent = component.render()
      return (
        <div class="editor-block style={blockStyle.value}>
          {RenderComponent}
        </div>
      )
    }

  }
})
```


`src/utils/ediotr-config.jsx`


```jsx
function createEditorConfig(){
  const componentList = []
  const componentMap = {}
  return {
    componentList,
    componentMap,
    registerConfig: (component) => {
      componentList.push(component)
      componentMap[component.key] = component
    }
  }
}


registerConfig.register({
  label: "文本",
  preview: () => "文本",
  render: () => "文本",
  key:"text"
})
registerConfig.register({
  label: "按钮",
  preview: () => <ElButton>按钮</ElButton>
  render: () => <ElButton>按钮</ElButton>
  key:"text"
})
registerConfig.register({
  label: "文本",
  preview: () => <ElInput>输入框</ElInput>,
  render: () =>  <ElInput>输入框</ElInput>
  key:"text"
})



export let registerConfig = createEditorConfig()
```


```js

const mousemove = (e) => {
  if (!dragState.dragging) {
    events.emit("dragstart")
  }
}


const keyboardEvent = (()=>{//初始化事件
  const onKeyDown = (e) => {
    // https://developer.mozilla.org/en-US/docs/Web/API/UI_Events/Keyboard_event_code_values
    // ctrlKey,altKey, metaKey, altKey
    // "Escape", "Enter", " ", "Tab"
    const {ctrlKey, key} = e
    let keyString = []
    if (ctrlKey) keyString.push("ctrl")
    keyString.push(key)
    keyString = keyString.join("+")
    state.commandList.forEach(({keyPress, name}) => {
      if(keyPress) return; //没有键盘事件
      if(keyboard === keyString){
        state.command[name]()
        e.preventDefault()
      }
    }
  }

  const init = () => { // 监听窗口中事件注册事件处理
    window.addEventListener('keydown', onKeyDown)
    return  () => {// 返回注册事件处理销毁函数
      window.removeEventListener('keydown', onKeyDown)
    }
  }
})()


    if (!el.contains.(e.target)){// 点击上下文菜单内部什么都不做
      setShowDropDown(false)
    }
```

## 组件的单选、多选、取消选中、全选

```js

let top = Infinity,
    left = Infinity,
    bottom = -Infinity,
    right = -Infinity

assembly.forEach(idx => {
  cmp = assembly.get(idx)
  top = Math.min(top, cmp.style.top)
  left = Math.min(top, cmp.style.left)
  bottom = Math.max(bottom, cmp.style.top + cmp.style.height)
  right = Math.max(right, cmp.style.left + cmp.style-width)
})

```

## 选中组件的拖拽


```ts

const onMousedown = (e) => {
  let startX = e.pageX;
  let startY = e.pageY;

  const onMousemove = throttle(（e => {
    const x = e.pageX;
    const y = e.pageY;

    const disX = x - startX;
    const disY = y - startY;

    updataAssemblyByDistance({disX, disY})
    startX = x;
    startY = y;
  }), 50)

  const onMouseup = () => {
    doucment.removeEventListener('mousemove', onMousemove)
    doucment.removeEventListener('mouseup',onMouseup)
  }

  doucment.addEventListener('mousemove', onMousemove)
  doucment.addEventListener('mouseup', onMouseup)
}
```


```ts
export const updataAssemblyByDistance({disX, disY}:Style){
  draft.assembly.forEach(idx => {
    const cmp = draft.cmps[idx]
    cmp.style.top += disY
    cmp.style.left += disX
  })
}


```


## 组件的伸缩

伸缩点绘制
```jsx
function StretchDots() {
  const positions = [
      ["top", "left"],
      ["top", "middle"],
      ["top", "right"],
      ["middle", "left"],
      ["middle", "right"],
      ["bottom", "left"],
      ["bottom", "middle"],
      ["bottom", "right"]
    ];

  // https://developer.mozilla.org/en-US/docs/Web/CSS/cursor
  // const positionStyleMap = {
  //     "top-left": "nwse-resize",
  //     "top-middle": "ns-resize",
  //     "top-right": "nesw-resize",
  //     "middle-left": "e-resize",
  //     "middle-right": "w-resize",
  //     "bottom-left": "nesw-resize",
  //     "bottom-middle": "ns-resize",
  //     "bottom-right": "nwse-resize"
  //   };


  return (
   // <div class="stretch-dot stretch-dot_top-left" data-direction="top, left"></div>
   {
    positions.map(position => {
      const positionKey = position.join('-')
      <div
        key={positionKey}
        class={`${styles.stretch-dot} stretch-dot_${positionKey} `}
        // data-direction={position.join(", ")}
        onMousedown={onMo}
        data-direction={positionKey.replace('-', ", ")}
      >
      </div>
      }
    )
   }


  )

}

```


`stretchDot.module.css`

```css
.stretch-dot_top-left {
    top: -8;
    left: -8;
    cursor: "nwse-resize"
}
.stretch-dot_top-middle {
    top: -8;
    left: 0;
    cursor: "n-resize"
}
.stretch-dot_top-right {
    top: -8;
    left: 8;
    cursor: "nesw-resize"
}
.stretch-dot_middle-left {
    top: 0;
    left: -8;
    cursor: "w-resize"
}
.stretch-dot_middle-right {
    top: 0;
    left: 8;
    cursor: "n-resize"
}
.stretch-dot_bottom-left {
    top: 8;
    left: -8;
    cursor: "nesw-resize"
}
.stretch-dot_bottom-middle {
    top: 8;
    left: 0;
    cursor: "s-resize"
}
.stretch-dot_bottom-right {
    top: 8;
    left: 8;
    cursor: "nwse-resize"
}

```


## 置顶、置底、上移、下移

```ts
//上移
const upZIndex = () => {
  [cmps[selectedIndex], cmps[selectedIndex + 1]] = [cmps[selectedIndex + 1], cmps[selectedIndex]]
  assembly[]
}
// 下移
const downZIndex = () => {}
// 置顶
const topZIndex = () => {}
// 置底
const bottomZIndex = () => {}

```


## 居中和边框对齐
拖拽组件时
距离值小于`showdiff = 12`时，显示对齐线，
距离值小于`snapdiff = 3`时， 就自动吸附

```ts
function onCmpDrag(distance, alignLineId, snapCb: () => void ) {
  const _distance = Math.abs(distance)
  const alignLine = document.getElementById(alignLineId)
  // 距离值小于`showdiff = 12`时，显示对齐线，
  if (_distance < showdiff) {
    alignLine.style.display = true
  }
  // 距离值小于`snapdiff = 3`时， 就自动吸附
  if (_disatance < snapdiff) {
    snapCb()
  }
}

```


```tsx

interface SnapLineProps {

}


export default function SnapLine() {

}

```






## 组件对齐


```ts

if(targetCmp.style.bottom === selectedCmp.style.top) {

}
```



```ts
const isCmpInView = (cmp:ICmpWithKey) => {
  const el = document.getElementById("cmp" + cmp.key)

  const viewHeight = window.innerHeight || document.documentElement.clientHeight
  const viewWidth = window.innerWidth || document.documentElement.clientWidth

  const {top, left, right, bottom} = el.getBoundingClientRect()
  return top>=0 && left >=0 && right <= viewWidth && bottom <= wiewHeight
}

```





## 补充知识

### white-space


|                | New lines | Spaces and tabs | Text wrapping | End-of-line spaces | End-of-line other space separators |
| :------------- | :-------- | :-------------- | :------------ | :----------------- | :--------------------------------- |
| `normal`       | Collapse  | Collapse        | Wrap          | Remove             | Hang                               |
| `nowrap`       | Collapse  | Collapse        | No wrap       | Remove             | Hang                               |
| `pre`          | Preserve  | Preserve        | No wrap       | Preserve           | No wrap                            |
| `pre-wrap`     | Preserve  | Preserve        | Wrap          | Hang               | Hang                               |
| `pre-line`     | Preserve  | Collapse        | Wrap          | Remove             | Hang                               |
| `break-spaces` | Preserve  | Preserve        | Wrap          | Wrap               |                                    |



## 显示缩略图，海报页面生成可下载图片

`Header/index.tsx`


```tsx
const saveAndDownload = () => {
  const {imgSrc} = result.thumbnail
  const downloable = document.createElement('a')
  downloable.href = imgSrc
  downloable.download = result.title + '.png'
  downloable.style.display = "none"
  document.appendChild(downloable)
  downloable.click()
  document.removeChild(downloable)
}
```

`vite.config.ts`


```ts
export default defineConfig(
  server: {
    "proxy": {
      "/api": "http://template.xiyuan.dev",
      "/static": "http://template.xiyuan.asset"
    }
  }
)
```


## 动画

```tsx

type Pair = {name: string, value: string | nubmer }
const handleStyleChange(
  _pair: Pair | Array<Pair>
) {
  const pair:Array<Pair> = Array.isArray(_pari) ? _pair : [_pair]
  const newStyle:Style = {}
  pair.forEach([name,value] => { newStyle[name} = value })
}

<Item label="循环次数" tip="s">
  <input
  type="number"
  value={parseInt(style.animationDuration)}
  onChange={e => handleStyle(e, {
    name: "animationDuration",
    value: e.target.value + 's'
  }}
  />
</Item>


```


```ts
interface ICmp {

  groupKey: string,
  groupCmpsKey: Arrary<string>
}


```
