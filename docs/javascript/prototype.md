![protype inheritance](images/jsobj_full.jpg)



- [x] 原型:任何js对象的创建都会关联一个对象, 任何根据原型的构造函数创建出来的对象,都会继承原型上的属性

```js
function (){
	  this = Object.create(prototype)
}

const o = new Object()
console.log(o.__proto__ === Object.prototype)
console.log(Object.getPrototypeOf(o) === Object.prototype)
console.log(Object.prototype.constructor === Object)



```

- [x] 原型链和原型继承

> behaviour delegation vs prototype inheritance

```js
console.log(o.constructor === Object)
console.log(Object.prototype.constructor === Object) 
```





## 手写call和apply

```js
Function.prototype._call = function(context, ...args){
  const symbol = Symbol()
  // set context obj to window if null
  context = Object(context|| window)
  context[symbol] = this
  // call the function with contxt obj
  // if it has return value, recevie and return
  const result = context[symbol](...args) 
  delete context[symbol];// delete the unique key
  return result
}


```

## 手写bind

```js
Function.prototype._bind = function(context){
  if(typeof this !== 'function'){
		throw Error('this is not a function')
  }
  // this is the `bind` was called on
	var _this = this 
	var args = Array.prototype.slice.call(arguments, 1)
  
  var fNOP = function {}
  
	var fbound =  function(){
			return _this.apply(
        // ignores the "explictly bound this" parameter when it's called as a constructor
        this instanceof fNOP ? this : context, 
        args.concat(Array.prototype.slice(arguments.1))
      )
    	// 避免fboud 修改原型的时候把this的原型也修改了
    	fNOP.prototype = this.prototype
			fbound.prototype = new fNOP()
    	return fbound
  }
}
```

### 手写new

```js
function objectFactory(){
  const o = new Object(null)
	var Constructor = [].shift.call(arguments)
  o.prototype = Constructor.prototype
	var ret = Constructor.apply(o,arguments)
  return typeof ret === 'object' ? ret : o
}
```



