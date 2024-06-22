# typescript deep dive

- scanner( scanner.ts)
- parser(parser.ts)
- binder(binder.ts)
- checker(checker.ts)
- aemitter(emitter.ts)





##  index signatrue

```ts
const propertyKey = String | Symbol | Number


// toString method will get called whenever the obj is used in an index position
const o = {
  toString(){
		console.log('toString Called')
  }
}


// default toString implementation on an object is pretty awful, e.g. on v8 it always returns [object Object]:





```



