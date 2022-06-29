// 首先我们来分析一下 Promsie 干了什么事情
Promise 对象里面是一个立即执行函数,然后立即执行函数带了两个状态回调函数,resolve 和 reject ,然后这两个回调函数最终也返回一个新的 promise 对象,以保证promise then 的链式调用

`
function Promise(fn) {
  this.callbackArray = []
  const resolve = (value) => {
     setTimeout(()=> {
        this.data = value
        this.callbackArray.forEach(callbackFn => {
           callbackFn(value)
        })
     })
  }
  fn(resolve)
}s
// promise 链式调用方法then
Promise.prototype.then = function(onResolve) {
   return new Promise((resolve) => {
      this.callbackArray.push(()=> {
          const res = onResolved(this.data)
          if(res instanceof Promise) {
             res.then(resolve)
          } else {
             resolve(res)
          }
      })
   })
}

`
