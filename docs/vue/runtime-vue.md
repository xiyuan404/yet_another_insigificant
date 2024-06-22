```ts
Object.prototype.toString.call({})// '[object Object]'
Object.prototype.toString.call(new Set()) // '[object Set]'
Object.prototype.toString.call(new Map())// '[object Map]'


const enum TargetType {
  INVALID = 0,
  COMMON = 1,
  COLLECTION = 2
}


function targetTypeMap(rawType: string){
  switch (rawType) {
    case 'Object':
    case 'Array':
      return TargetType.COMMON
    case 'Map':
    case 'Set':
    case "WeakMap":
    case "WeakSet":
      return TargetType.COLLECTION
    default: 
      return TargetType.INVALID
  }
}
```





```ts
// length-altering mutation methods to avoid length being tracked
(['push','pop','shift','unshift', 'splice']) as const
```







![insert_before](images/insert_node.svg)



### VDOM

__Vue 2 API__

```ts
render(h){
  return h('div',{
    attrs: {
      id: 'foo'
    },
    on: {
      click: this.onClick
    }
  }, 'hello')
}
```

__Vue 3 API__

- Flat props Structure
- Globally imported `h` helper


```ts
render(h){
	return h('div',{
    id: 'foo',
    onClick: this.onClick
  }, 'hello')
}

```

- [x] slot

```ts
render(){
  const slot = this.$slots.default
 		? this.$slots.default()
  	: []
	return h('div',{class: 'stack'}, slot.map(child=>{
    return h('div', { class: `mt-${this.$props.size}`}, [child])
  }))
}
```

- [x] v-if

```ts
render(){
  let nodoToReturn
  nodeToReturn = this.firstConditon
  	? h('div', { id: 'hello' }, [h('span', 'world')])
  	: this.otherCondition
  		? h('p', 'other branch')
  		: h('span')
}
```

- [x] v-for

```ts
render(){
  return this.list.map(item => {
    return h('div', { key: item.id }, item.text )
  })
}
```



```ts


function mount(vnode, container){
  const el = vnode.el = document.createElement(vnode.tag)
  for(const key in vnode.props){
    const value = vnode.props[key]
    if(key.startsWith('on')){
      el.addEventListener(key.slice(2).toLowerCase())
    } else {
      el.setAttribute(key, value)
    }
  }

  if (vnode.children){
    if(typeof vnode.children === 'string'){
      el.textContent = vnode.children
    } else {
      vnode.children.forEach(child => {
        mount(child, el)
      })
    }
  }
  container.appendChild(el)
}

```







```ts
function normalizeStyle(el, prevValue, nextValue) {
  // {style:{color:red}} -> {style: {backgroundColor: 'red'}}
  const style = el['style']
  for (const key in nextValue){
    style[key] = newxValue[key]
  }
  for (const key in preValue) {
    if(!nextValue[key]){
			style[key] = null
    }
  }
    
}
```

## bitwise flags


````markdown
BINARY DECIMAL COLOR
------ ------- -----
   001       1  Red
   010       2  Green
   011       3  Red+Green
   100       4  Blue
   101       5  Blue+Red
   110       6  Blue+Green
   111       7  Blue+Green+Red
   
```ts
	enum Color {
		Red = 1 << 0
		Green = 1 << 1
		Bule = 1 << 2
	}
	
```
````
- [x] composable

```ts
Color purple = Red | Blue
Color yellow = Green | Blue 
```

- [x] find intersection  or membership

```ts
const hasRed =  !!(purple & Red)  // false
const hasGreen = !!(purple & Green) // false
const iestColor = purple & yellow // blue
```

```ts
const PermissionExecute = 1 << 0
const PermissionRead = 1 << 1
const PermissionWrite = 1 << 2

const PermissionAll = PermissionExecute | PermissionRead | PermissionWrite

class PermissionManager {
  constructor(){
    this.permissions = 0
  }
  set(flag){
    this.permissions |=  flag 
  }
  clear(flag){
    this.permissions &= ~flag 
  }
  hasAll(flag) {
    return (this.permissions & flag) === flag
  }
  hasOneOf(flag){
    return (this.permissions & flag) !== 0
  }
}


/*========== Test Code ========*/
 const manager = new PermissionManager();

    // 设置所有权限
    manager.set(PermissionAll);

    // 清除读取权限
    manager.clear(PermissionRead);

    // 检查是否同时具有执行和读取权限
    const hasExecuteAndRead = manager.hasAll(PermissionExecute | PermissionRead);

    // 检查是否同时具有执行和写入权限
    const hasExecuteAndWrite = manager.hasAll(PermissionExecute | PermissionWrite);
```



```ts
export const enum ShapeFlags {
  ELEMENT = 1,
  FUNCTIONAL_COMPONENT = 1 << 1,
  STATEFUL_COMPONENT = 1 << 2,
  TEXT_CHILDREN = 1 << 3,
  ARRAY_CHILDREN = 1 << 4,
  SLOTS_CHILDREN = 1 << 5,
  TELEPROT = 1 << 6,
  SUSPENT = 1 << 7,
  COMPONENT_SHOULD_KEEP_ALIVE = 1 << 8,
  COMPONENT_KEPT_ALIVE = 1 << 9,
  COMPONENT = ShapeFlags.STATEFUL_COMPONENT | ShapeFlags.FUNCTIONAL_COMPONENT
}
```



## built-in component



