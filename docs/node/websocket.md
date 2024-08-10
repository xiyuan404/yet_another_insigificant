## server sent event


服务端
```js
let sendCount = 1
app.get('/news-feed', (req,res) => {
  const SseStream = require('ssestream')
  res.header("Content-Type", "text/event-strem")
  const sseStream = new SseStream(req)
  sseStream.pipe(res)
  const pusher = setInterval( () => {
    sseStream.write({
      id: sendCount++,
      event: 'message',
      data: {ts: new Date().toTimeString()}
      retry: 20000,
    })
  }, 1000)

  res.on('close', ()=> {
    clearInterval(pusher)
    sseStream.unpipe(res)
    )
})


```
