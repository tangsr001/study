babel 是如何编译 ES6 等其他高版本的js 的
constructor 
es6中：
`
  class Person{
      constructor(name){
          this.name = name;
      }
      sayHello(){
          return `hello ,I an ${this.name}`
      }
  } 
  let kevin = new Person('kevin')
  kevin.sayHello() // hello ,I an kevin
`
再来看ES5 
`
  function Person(name){
        this.name = name;
  } 
  Person.prototype.sayHello = function(){
      return `hello , I am ${this.name}`
  }
`
我们可以看到 ES5 的构造函数就相当于 ES6 的 constructor (构造器)
需要注意的地方是 类的内部所定义的方法，都是不可枚举的
以上的例子ES6
` 
  Object.keys(Person.prototype) //[]
  Object.getOwnPropertyNames(Person.prototype) // ['constructor','sayHello']
`
在ES5 中
`
  Object.keys(Person.prototype) //['sayHello']
  Object.getOwnPropertyNames(Person.prototype) //['constructor','sayHello']
`
实例属性
以前，我们定义实例属性，只能写在类的 constructor 方法里面，比如
`
  class Person{
     constructor(){
         this.state ={
             count:0
         }
     }
  }
`
然而现在有一个提案，对实例属性和静态属性都规定了新的写法，babel 已经支持
现在我们可以在 ES6这样写

`
  class Person{
      this.state ={
          count:0
      }
  }
`

对应到 ES5 就是

`
  function Person(name){
      this.state = {
          count:0
      }
  }
`

静态方法
在所有的类中定义的方法，都会被实例继承，如果在一个方法前，加上一个static 关键字，就表示不会被实例继承，而是直接通过实例调用，这种方法就称之为'静态方法'

ES6 静态方法
`
  class Person{
      static sayHello(){
          return 'hello'
      }
  }
  // 调用
  Person.sayHello()
  var kevin = new Person()
  kevin.sayHello() // 这里不会被继承，会报错
`

对应ES5 就是
`
  function Person(){}
  Person.sayHello = {
      return 'hello'
  }
  var kevin = new Person()
  kevin.sayHello() // 这里也会报错
`
new 的调用
值得注意的是：类必须使用 new 调用，否则会报错，这是它跟普通构造函数的一个主要区别，后者不用new 也能执行

