call() 方法在使用一个指向的 this 的和若干个指定的参数的前提下调用某个函数或方法
` 
  var foo = {
    value:1
  }
  function bar(){
    console.log(this.value)
  }
  bar.call(foo)  // 1
`
上述代码干了两件事
1.通过call 改变了bar 的this 指向
2.执行了bar函数


我们可以通过对象上添加属性也能实现
` 
  var foo = {
     value:1,
     bar:function(){
       console.log(this.value)
     }
  }
  foo.bar() //1
`
实现思路,我们可以先添加属性,然后再改变属性,最后再删除属性
实现第一版
`
   // bar.call(foo)
   Function.prototype.call = function(context){
     // 这里的content 类似foo
     context.fn = this  // 改变 this 指向
     context.fn() //执行
     delete context.fn
   }
`
下面我们来实现 传参和没有传值为 null 
`
  Function.prototype.call2 = function (context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 0, len = arguments.length; i < len; i++) {
        args.push(arguments[i]);
    }
    var newArray =  args.slice(1)
    console.log(...newArray)
    var result = context.fn(...newArray)
    delete context.fn
}
`
接下来我们来实现 apply ,直接一步到位

`
  Function.prototype.apply2 = function(context){
     let context = context || window
     context.fn = this
     let parmArray = []
     if(context.slice(1,2).length>0){
        parmArray = context.slice(1,2)
        context.fn(...parmArray)
     }esle{
        context.fn()
     }
     delete context.fn()
  }
`


最后我们来实现一下 bind()
`
       Function.prototype.newBind = function(){
        // 目标对象  
        var target = arguments[0] || window
        // 构造函数的 this 环境
        var self = this;
        
        //newBind 的形参数列表，去掉target。
        var args = [].slice.call(arguments,1);
        var temp = function(){};
        
        // 创建一个新函数 newFn
        var newFn = function(){
            // 新函数的参数列表
            var _args = [].slice.call(arguments,0);
            //判断执行函数是否是构造函数执行的，是的话就用this，
            //若不是通过new的方式来执行，而是直接执行的话，就用target
            return self.apply(this instanceof temp ? this : target, args.concat(_args));
        }
        
        //让this(-->f show(){})和newFn形成关联，确保原型链不被破坏。
        temp.prototype = this.prototype;
        // 保证返回的函数this 指向 target
        newFn.prototype = new temp();

        //返回新函数
        return newFn;
    }
`



