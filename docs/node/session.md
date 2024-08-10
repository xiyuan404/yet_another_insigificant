


## session的实现

```js
const SESSION_KEY = 'connect.sid'
const sessions = {}

app.get('/', (req,res)=>{
  const sessionId = req.cookies[SESSION_KEY]
  if(sessionId){
    //
  } esle {
    getUUID()
  }
})

const getUUID = () => {
  let count = 0
  return () => {
    return (++count).toString()
  }
}()


```

```js
// 8-4-4-4-12
export const getUUID() {
  [...crypto.getRandomValues(new Uint8Array(16))].map(b => b.toString(16).padStart(2,'0').join('-').replace(/(.{8})(.{4})(.{4})(.{4})(.{12})/, '$1-$2-$3-$4-$5');
}

```
