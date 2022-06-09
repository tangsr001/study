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


