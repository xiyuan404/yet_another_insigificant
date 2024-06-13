## “Methods and the special variable this”

the receiver of the method call and assigned to the special variable this
method call on object

## “Property attributes and property descriptors [ES5]”

### Use case for spreading: default values for missing properties

```js
  const default = {alpha: 'a', beta: 'b'};
  const providedData = {alpha: 1};

  const allData = {...default, ...providedData};
  assert.deepEqual(allData, {alpha: 1, beta: 'b'});
```

## this pitfall: accidentally shadowing thi

```js
const prefixer = {
  prefix: '==>',
}
```

## optional chaining for inconsistent property getting

Handling defaults via nullish coalescing

```js
const streetNames = presons.map((p) => p.address?.street?.name ?? '(no name)')
```

expressions equivalent

```js
const isCallable = o =>
function polyfillOption(o){
  if(isCallable(o)){

  }
o?.prop
(o !== undefined && o !== null) ? o.prop : undefined // Optional fixed property getting

o?.[«expr»]
(o !== undefined && o !== null) ? o[«expr»] : undefined // Optional dynamic property getting

f?.(arg0, arg1)
(f !== undefined && f !== null) ? f(arg0, arg1) : undefined //  Optional function or method call
}
```

### The in operator checks if an object has a property with a given key:

It returns a copy of object where the keys and values of all properties are swapped:

```js
function invert(object) {
  const reversedEntries = Object.entries(object).map(([key, value]) => [
    value,
    key,
  ])
  return Object.fromEntries(reversedEntries)
}

assert.deepEqual(invert({ a: 1, b: 2, c: 3 }), { 1: 'a', 2: 'b', 3: 'c' })
```

It returns a copy of object that has only those properties whose keys are mentioned in the trailing arguments:

```js
function pick(object, ...keys) {
  const filteredEntries = Object.entries(object).filter(([key, _value]) =>
    keys.includes(key)
  )
  return Object.fromEntries(filteredEntries)
}
```

```js
function fromEntries(iterable) {
  const result = {}
  for (const [key, value] of iterable) {
    let coercedKey
    if (typeof key === 'string' || typeof key === 'symbol') {
      coercedKey = key
    } else {
      coercedKey = String(key)
    }
    result[coercedKey] = value
  }
  return result
}
```

- A property key can be either a string or a symbol.
- A property name is a property key whose value is a string.
- A property symbol is a property key whose value is a symbol.


 // creating an object
{}//object literal
//generate object using constructor function


//problem with this approach: copy identical function and data on every single object, insted we wanna share function
//avoid duplication with prototype 


//object link to other object
// proto chain look up

// userEuclid.__proto__ -> paidUserFuncStore{}
userEuclid = Obejct.create(paidUserFuncStore)//delegated object





///===================
// subclassing with factory function
const userFuncStore = {
  logIn:()=>{ return 'succeed logging in'},
  logOut:()=>{ return 'succeed logging out'}
}

const paidUserFuncStore = {
  exclusive:()=>{ return 'premium only'},
  analytics:()=>{return 'datav for premium'},
  techSupport:() => {return 'all-time support'}
}


function UserFactory(name, email){
  const newUser = Object.create(userFuncStore)
  newUser.name = name 
  newUser.email = email
  return newUser
}

// creating an object using sub-factory function
function PaidUserFactory(paidName, paidEmail, accountBalance){
  const newPaidUser = UserFactory(paidName, paidEmail)
  Object.setPrototypeof(newPaidUser, paidUserFuncStore)
  newPaidUser.accountBalance = accountBalance
  return newPaidUser
}

//paidUserFuncStore.__proto__ -> userFunctionStore
Object.setPrototypeOf(paidUserFuncStore, userFuncStore)
const paidUser1 = PaidUserFactory('Susis', 'susis@gmail.com', 20)



///===================
// subclassing with new and call


// 1. create an object with new keyword


// 2. create a subclass with constructor

function baseUserFactory(name, email){
  this.name = name 
  this.email = email
}

function baseUserFactory.prototype = {
  logIn: () =>{},
  logOut: ()=>{}
}

// delegate functionality to baseUser
PaidUserFactory.prototype = Object.create(UserFactory.prototype)

// add paidUser only functionality
PaidUserFactory.prototype.exclusive = function(){
  this.payWallUnlocked = true
}

function paidUserFactory(paidName, paidEmail, accountBalance){
  baseUserFactory.call(this, paidName, paidEmail)
  this.accountBalance = accountBalance
}

const paidUser2 = new paidUserFactory('odin','odin@gmail.com', 10)





/*  new keyword
do the dirty work for us
1.create a new object assign to this
2. set its __proto__ to construct.prototype
2.return the new object
*/

//


//all object be default link to



/* 
In the spec it technically call this __proto__ property[[prototype]] with two square brackets around it
*/



/// ========================
// this bindings
// .call() .apply allow us control the assignment of this 

/*this keyword scoping issue
this determined entirely by how the function was called
_this = this
that = this
arrow function lexical this 
this always refers to the object it was call on on
*/


//1. create an object with class keyword

//2. create a subclass with extends keyword


//3. creating an object with subclass


//4. using super in a subclass constructor

class paidUser extends baseUser{
  constructor(paidName, paidEmail, accountBalance){
    Super(paidName, paidEmail)//Reflect.construct(baseUser,args,paidUser)
  }
}