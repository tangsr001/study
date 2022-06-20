手写实现一个 async 函数
` function asyncToGenerator (generatorFunc) {
    // 返回一个新的函数
    return function() {
      // 先调用 generator 生成迭代器
      gen = generatorFun.apply(this,arguments)
      // 返回一个 promise 因为外部是用.then() 的方式,或者 await 的方式去使用这个函数的返回值
      return new Promise((resolve,reject) => {
        // 内部定义一个 step 函数,用来一步步的跨国 yield 的阻碍
        // key 有 next 和 throw 两种取值,分别对应 gen 和 next 和 throw 方法
        // arg 参数是用来把 promise resolve 出来的值交给下一个 yield 
        function step(key,arg) {
          let generatorResult = gen[key](arg)
          try{
            generatorResult = gen[key](arg)
          }catch(error) {
            return reject(error)
          }
          const {value, done} = generatorResult
          if(done) {
            return resolve(value)
          }else{
            return Promise.resolve(value).then(
               function onResolve(val) {
                 step('next',val)
               }
               function onReject(err) {
                 step('throw',err)
               }
            )
          }
        }
        step('next')
      })
    }
 }
` 
