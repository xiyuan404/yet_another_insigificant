## property flag and descriptors

### property flags

`writable` – if true, the value can be changed, otherwise it’s read-only.
`enumerable` – if true, then listed in loops, otherwise not listed.
`configurable` – if true, the property can be deleted and these attributes can be modified, otherwise not.


### To get those flag


`Object.getownPropertyDescriptor(o, propName)`

## To change the flags

`Object.defineProperty(obj, propertyName, descriptor)`
```js
Object.defineProperty(user, "toString", {
  enumerable: false
});

for (let key in user) {}
Object.keys(user).forEach()
```



### Object.freeze()

new properties cannot be added, existing properties cannot be removed
or value cannot be changed, and the object's prototype cannot be re-assigned


## proxy and reflect



## map and set
