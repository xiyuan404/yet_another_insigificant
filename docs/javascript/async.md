

## 异步业务场景

- 网络请求
- 定时任务 (验证码countdown, toast timeelapsed fade, request timeout cancel)
- 事件绑定
- 大数据处理 web worker

## 处理策略

- 回调函数

- promise

- async/await

- 分发订阅模式

  ```js
  class EventEmitter {
    constructor(){
      this._emits = {}
    }
    on(eventName, cb){
      const cbs = this._emits[eventName] || []
      cbs.push(cb)
      this._emits[eventName] = cbs
    }
    
    emit(eventName, ...args){
      this._emits[eventName].forEach(cb=> cb(...args))
    }
  
    off(eventName, cb){
      const cbs = this._emits[eventName] || []
      const newCbs = cbs.filter(effect => effect != cb)
      this._emits[eventName] = newCbs
    }
  
    once(eventName,cb) {
      const one = (...args) => {
        cb(...args)
        this.off(eventName,one)
      }
  
      this.on(eventName, one)
    }
  }
  ```

  

- generator 函数

- promise.all

- 预加载资源

- event bus

  