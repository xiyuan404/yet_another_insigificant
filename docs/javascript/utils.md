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
