---

---
# for statement

used for control flow
for(
  initation; 
  condition; 
  afterthought
  )
  block statement;
  {}

## execution order
initation->condition->block statement->afterthought

## initialization
1. initialization
variable get declared and assign value fitst time.
  - declared while assing value
  - assign value after delcaring variable
2. unitialization
variable get declared but did not assign value yet.
=== "declar with var"

    ```js
      for (var i = 0; i<3; i++){
        console.log(i)
        }
      console.log(i)
    ```

=== "declar with let"

      ```js
        for (var i = 0; i<3; i++){
        console.log(i)
        }
        console.log(i)
      ```
## condition
conditon => false => exit loop, goto the first statement/expression after for
  empty expression => false 
condition => ture => enter loop


## afterthough
  An expression to be evaluated at the end of each loop iteration
   occurs before the next evaluation of condition



## lexical scope
_for with web api_

```js
  for(var i = 0; i < 3; i++) {
    setTimeout(() => {
      console.log(i)
    },500)
  }

```

```js
  const marcoTaskQueue = [
    () => {console.log(i)}, //500
    () => {console.log(i)}, //1000
    () => {console.log(i)}, //1500
  ]

  const callstack = [
    () => {console.log(i)},
    console.log(i) // variable binding lookup to 3
  ]
```

_for statement with let variable declared_
let declarations are special-cased by for loops 
if initialization is a let declaration,
A new lexical scope is created for every loop
The binding values from the last iteration are used to re-initialize the new variables.
afterthought is evaluated in the new scope.

```js
const arr = []
  for(let i = 0; i < 3; i++ ){
    const fn = () => {
      console.log(i)
    }
    arr.push(fn)
  } 

  arr.forEach(fn => fn())

```
_equivalent to_

```js
const arr = [];
let initialStep = 0,
  lastStep = initialStep;

{
  let i = lastStep; //0
  const fn = () => {
    console.log(i);
  };
  arr.push(fn);
  lastStep++;
}
{
  let i = lastStep; //1
  const fn = () => {
    console.log(i);
  };
  arr.push(fn);
  lastStep++;
}
arr.forEach((fn) => fn());

```