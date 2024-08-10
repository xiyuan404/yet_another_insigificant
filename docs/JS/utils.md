## Array

array construct

## array fill

> ### [Parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#parameters)
>
> - [`value`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#value)
>
>   Value to fill the array with. Note all elements in the array will be this exact value: if `value` is an object, each slot in the array will reference that object.
>
> - [`start`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#start) Optional
>
>   Zero-based index at which to start filling, [converted to an integer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#integer_conversion).Negative index counts back from the end of the array — if `-array.length <= start < 0`, `start + array.length` is used.If `start < -array.length` or `start` is omitted, `0` is used.If `start >= array.length`, no index is filled.
>
> - [`end`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#end) Optional
>
>   Zero-based index at which to end filling, [converted to an integer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number#integer_conversion). `fill()` fills up to but not including `end`.Negative index counts back from the end of the array — if `-array.length <= end < 0`, `end + array.length` is used.If `end < -array.length`, `0` is used.If `end >= array.length` or `end` is omitted, `array.length` is used, causing all indices until the end to be filled.If `end` implies a position before or at the position that `start` implies, nothing is filled.
>
> ### [Return value](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill#return_value)
>
> The modified array, filled with `value`.



```js
// fill(value)
// fill(value, start)
// fill(value, start, end)

function customFill(value, start, end) {
  let actualStart,actualEnd;

  if((start !== undefined && isNaN(start)) || (end !== undefined && isNaN(end))) {
		return this
  }

	if (!start) {
		actualStart = 0
  } else if (start < -array.length)) {
		actualStart = 0
  } else if (start < 0) {
    actualStart = this.length + start
  } else if (start >= this.length){
			return this
  } else {
    actualStart = start
  }

  	if (!end) {
		actualEnd = 0
  } else if (end < -array.length)) {
		actualStart = 0
  } else if (end < 0) {
    actualStart = this.length + end
  } else if (end >= this.length){
			return this
  } else {
    actualEnd = this.length
  }

    for (let i = actualStart; i < actualEnd; i++){
    this[i] = value;
  }
  return this;


}
Array.prototype._fill = customFill
```

usecase: fill the board

```js
Array(9).fill(null)
```

length-altering mutation methods

- push
- pop
- shift
- unshift
- splice


## lyric format
```js
const parseLyric = (lrcRaw) => {
  const lrcLines = lrc.split('\n')
  const lrcParsed = []
  // /[01:22:550]/
  const extractTimestampRegex = /\[(?<min>\d{2}):(?<sec>\d{2})(?:\.|:)(?<ms>\d{2,3})\]/
  for (const line of lrcLines.trim()) {
    const match = line.match(timeStampRegex)
    !match && continue
    const {min,sec,ms} = match.groups
    const time = Number(min)*60 + Number(sec) + Number(ms) * .001
    const content = match.replace(extractTimestampRegex,"").trim()
    lrcParsed.push({time, content})
  }
}

```


## time format

```js
// yyyy/MM/dd hh:mm:ss
// hh:mm:ss yyyy/MM/dd
// yyyy-MM-dd hh.mm.ss
// hh:mm:ss
const formatTime = (timestamp, format) => {
    const date = new Date(timestamp)
  const timeCmps = {
    'y+': date.getFullYear(),
    'M+': date.getMonth() + 1,
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds(),
  }
  for(const pattern in timeCmps) {
    const patternRe = new Regex(pattern)
    if(patternRe.test(format)){
      const val = (timeCmps[key] + "").padStart(2,"0")
      format.replace(patternRe, val)
    }
  }
  return format
}

```


## 防抖
输入联想
事件触发时，响应函数不会立即执行，而是等待一段时间
当事件密集频繁触发时，函数的执行会被频繁延迟
等待一段时间后还没有事件触发，才会真正执行响应函数


- [x] 核心实现:防抖(延迟和取消上次)
- [x] 参数绑定: this和args
- [x] 取消功能
- [x] 立即执行
- [x] 获取返回值
- [x] 封装独立函数

```js
const debounce = (exec,delay, immediate = false) => {
  // this is deterimined at call time
  // inputEl.oninput() this = inputEl
  let timer = null
  let isInvoke = false



  function _debounce(...args){// argument&this binding
    return new Promiser((resolve, reject) => {
      timer && clearTimeout(timer)

      let result = null

      if(immediate && !isInvoke) {
        result = exec.apply(this,args)
        resolve(result)
        isInvoke = false
        return
      }

      timer = setTimeout(()=> {
        result = exec.apply(this,args)
        resolve(result)
        timer = null
        isInvoke = false
      },delay)
    })
  }
  // 取消功能
  _debounce.cancel = () => {
    timer && clearTimeout(timer)
    timer = null
    isInvoke = false
  }

  return  _debounce
}

export default debounce
```



## 节流

- [x] 核心实现
- [x] 参数绑定: this & args
- [x] 立即执行控制
- [x] 尾部执行控制
```js
const throttle = (exec, interval, {lead = false, trail = false} = {}) => {
  let startTime = 0 // 记录上次执行的结果
  let timer = null


  function _throttle (...args){
    // 每次执行重新计算
    const currTime = Date.now().getTime()

    // 立即执行控制
    if(!lead && startTime === 0) {
      startTime = nowTime
    }

    const timeSpan = currTime - startTime

    if(timeSpan >= interval){
      timer && cleatTimeout(timer)
      exec.apply(this, ...args)
      startTime = currTime
      return
    }

    if (!trail && !timer) {
      const leftTime = interval - timeSpan
      timer = setTimeout(()=>{
        exec.apply(this,args)
        startTime = new Date().getTime()
        time = null
      },leftTime)
    }
  }

  return _throttle
}

```




## 深拷贝

- [x] fix self reference: Range Error: maxmium call stack exceeded

```js
function isObject(val){
  const valType = typeof val
  return (val !== null) && (valType === 'object' || valType === 'function')
}


const deepClone = (origin, hashMap = new WeakMap) => {
  // value is symbol(remains unique throughout the lifetime of the program. )
  if (typeof origin === 'symbol' ) return Symbol(origin.description)
  // valType: string, num, boolean (primitive)  or function
  if (!isObject(origin) || typeof origin === 'function') return origin


  //
  if(hashMap.has(origin)) return hashMap.get(origin)
  const target = Array.isArray(origin) ? [] : {}
  hashMap.set(origin, target)
  for (const key in origin) {
    target[key] = deepClone(origin[key], hashMap)
  }
  const symbolKeys = Object.getOwnPropertySymbols(origin)
  for (const key of symbolKeys) {
    target[Symbol(key.description)] = deepCopy(origin[key])
  }

  return target
}

```

## 事件总线

观察者模式，三个角色
发布者(Publisher):发出事件event
订阅者(Subscriber): 订阅事件（event)，并会进行响应
事件总线(EventBus):

### 实现事件总线的第三方库
mitt
### 实现自己的事件总线
事件的监听方法on
事件的发射方法emit
事件的取消监听方法off

> 作为工具库的实现，前缀name-colision

```js
 class EventBus {
   constructor(){
     this._emits = {}
   }
   on(eventName, cb){
     const handlers = this._emits[eventName] || []
     handlers.push(cb)
     this._emits[eventName] = handlers
   }

   emit(eventName, ...args){
     this._emits[eventName].forEach(cb=> cb(...args))
   }

   off(eventName, cb){
     const cbs = this._emits[eventName] || []
     const newCbs = cbs.filter(effect => effect != cb)
     this._emits[eventName] = newCbs
   }
```

## axios请求二次封装


`request.ontimeout`

`request.onabort`
