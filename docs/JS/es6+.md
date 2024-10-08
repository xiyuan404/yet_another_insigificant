

- [spider monkey team](https://spidermonkey.dev/)
- [v8 team](https://v8.dev/)
- [webkit team](https://webkit.org/team/)



# es6+, new features in Javascript

- [x] imports and exports
- [x] Arrow functions
- [x] Classes
- [x] Destrcturing
- [x] Spread
- [x] Async/ await





## class

```js
function _defineProperties(target, props){
	for(var i = 0; i < props.length; i++){
    var descriptor = props[i]
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if(value in descriptor) descriptor.writable = true;
    Object.defineProperty(target, _toPropertyKey(descriptor.key), descriptor)
  }
}

function _toPrimitive(input, hint){
	if(typeof input !== 'obejct' || input === null ) return input
  var prim = input[Symbolo.toPrimitive]
  if(prim!==undefined){
    var res = prim.call(input, hint || "default")
    if(typeof res !== 'object') return res
    throw new TypeError("@@toPrimitive must return a primitive value")
   }
  return (hint === 'string' ? String : Number)(input)
}

function _toPropertyKey(arg){
  var key = _toPrimitive(arg, "string")
  return typeof key === 'symbol' : key : String(key)
}



function _createClass(Constructor, protoProps, staticProps){
	protoProps && _defineProperties(Constructor.prototype, protoProps);
  staticProps &&  _defineProperties(Constructor, staticProps)
  Object.defineProperty(Constructor, "prototype", {writable: false})
  return Constructor
}

function _classCallCheck(instance, Constructor){
  if(!(instance instanceof Constructor)){
    throw new TypeError("Cannot call a class as a function")
  }
}

var Person = /*#__Pure__#*/ _createClass(function Person(){
	_classCallCheck(this, Person)
})
```

gist

```js
const saferHTML = str =>
	return str.replace(/&/g, "&amp;")
						.replace(/</g, "&lt;")
						.replace(/>/g, "&gt;")
```





## 模版字符串





> __expression__
>
> - member expression
> - call expression
> - name
> - literal:
>  - object literal: `[]` `{}`
   - pattern literal: `/pattern/`
   - number literal: `0` `0x0` `0b0`
   - string literal: `''`
> - jsx

## 对象展开

```js
function ownKeys(object, enumerableOnly){
  // Object.keys() returns an array whose elements are strings corresponding to the enumerable string-keyed property names found directly upon object.
  var keys = Object.keys(object)

  if(Object.getOwnPropertySymbols){
    // get all symbol properties of a given object as an array of symbols
    var symbols = Object.getOwnPropertySymbols(object)
    if(enumerableOnly){
      symbols = symbols.filter(function(sym){
        return Object.getOwnPropertyDescriptor(object, sym).enumerable
      })
    }
    keys.push.apply(keys,symbols)
  }
  return keys
}



function _objectSpread(target){
	for(var i = 1; i < arguments.length; i++){
	var source = null != arguments[i] ? arguments[i] : {}
  if (i % 2) {
    ownKeys(Object(source), true).forEach(function(key){
      _defineProperty(target, key, source[key])
    })
  } else if (Object.getOwnPropertyDescriptors){
    Object.definePorperties(target, Object.getOwnPropertyDescriptors(source))
  } else {
    ownKeys(Object(source).forEach(function(key){
      Object.defineProperty(
      	target,
        key,
        Object.getOwnPropertyDescriptor(source,key)
      )
    }))
  }
  return target;

}
}

function _defineProperty(obj, key, value){
  if (key in obj) {
    Object.defineProperty(obj, key, {
      vlaue: value,
      enumerable: true,
      configurable: true,
      writable: true
    }) else {
      obj[key] = value
    }
  }
  return obj
}


var a = {
  a: 1
};
var b = {
  b: 2
};


var c = _objectSpread(_objectSpread({},a), b)
```



## Array polyfill

```ts
function includes
indexof
lastIndexof
pop
shift
push
unshift
splice
```



## Array utility

[underscore](https://github.com/jashkenas/underscore/blob/1abc36c169947c54c97e266513b1d763d0198f46/modules/_flatten.js)

[loadash]()(https://lodash.com/docs/4.17.15#compact)

## 编译工具

- babel
- esbuild
- swc

- rollup

### 打包工具

- webpack
- vite
