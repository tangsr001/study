new 操作符可以帮助我们构建出一个实例，并且绑定上 this
new 官方文档说明
js new的作用
模拟实现 new 关键字

new 的作用：
new 运算符允许开发人员创建用户定义对象类型或者具有构造函数的内置对象类型之一的实例
简单来说就是 new 运算符 用来创建对象或者创建构造函数的实例
`
function Car(make, model, year) { 
  this.make = make;
  this.model = model;
  this.year = year;
}
const car1 = new Car('Eagle', 'Talon TSR', 1993)
console.log(car1.make)

`
上述打印会输出 "Eagle" 

`语法 new constructor[([arguments])]`

参数
constructor 指定对象实例类型的类或函数

arguments  constructor 构造时调用的参数表

描述

new 关键字执行以下操作

1. 创建一个空对象
2. 将属性添加到 _Porto_ 链接到构造函数的原型对象的新对象
3. 将新创建的对象实例绑定为 this 上下文，执行构造函数的代码，为这个新对象添加属性
4. 判断函数的返回值类型，如果是值类型，返回创建的对象，如果是引用类型，就返回这个引用类型的对象

创建用户定义的对象需要两个步骤
1. 通过编写一个指定其名称和属性的函数来定义对象类型，例如，你用于创建对象的函数 Foo 可能如下所示
` function Foo(bar1, bar2){
    this.bar1 = bar1;
    this.bar2 = bar2;
}`
2. 用于创建对象的实例 new
`   var myFoo = new Foo('Bar 1',2021)`
执行代码时 new Foo(...),会发生以下情况
(1). 创建了一个新对象，继承自Foo.prototype 
(2). 使用指定的参数调用构造函数Foo,并把 this 绑定到新创建的对象上， new Foo 邓健月 new Foo() 即如果没有指定参数列表，则在没有参数的情况下调用Foo
(3). 构造函数返回的对象（不是 null、false、3.1415 或其他原始类型）成为整个new表达式的结果。如果构造函数没有显式返回对象，则使用在步骤 1 中创建的对象（通常构造函数不返回值，但如果他们想要覆盖正常的对象创建过程，他们可以选择这样做）

下面我们来实现new

我们先来看个例子
`function Otaku (name, age){
   this.name = name;
   this.age = age;
   this.habit = 'Games' 
}
// 因为缺乏锻炼的缘故，身体强度让人担忧
Otaku.prototype.strength = 60 ;

Otaku.prototype.sayYouName = function(){
  console.log('i am' + this.name)
}
var person = new Otaku('kevin',18)
console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // 60
`
从上面的例子中我们可以看到，实例可以访问 构造函数的属性，也可以访问原型上的属性

现在我们先来实现
` function objectFactory(){
  var obj = new Object();
  Constructor = [].shift().call(arguments)  // 这里数组转化成数组对象，并获取第一个参数，这里的参数为构造函数
  obj._proto_ =  Constructor.prototype   // 把空对像的原型指向构造函数的原型
  Constructor.apply(obj,arguments) //指定this 为obj
  return obj
}

var person = objectFactory(Otaku, 'Kevin', '18')

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // 60

person.sayYourName(); // I am Kevin
`

这是没有返回值的情况
我们来看有返回值的情况
` function Otaku (name, age) {
    this.strength = 60;
    this.age = age;

    return {
        name: name,
        habit: 'Games'
    }
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // Kevin
console.log(person.habit) // Games
console.log(person.strength) // undefined
console.log(person.age) // undefined

`

再看一下，返回值时基础类型的情况

` function Otaku (name, age) {
    this.strength = 60;
    this.age = age;

    return 'handsome boy';
}

var person = new Otaku('Kevin', '18');

console.log(person.name) // undefined
console.log(person.habit) // undefined
console.log(person.strength) // 60
console.log(person.age) // 18
`
所以还需要判断返回的值不是我们一个对象，如果是一个对象，我们就返回这个对象，如果没有，我们该返回什么就返回什么
` 
function objectFactory() {

    var obj = new Object(),

    Constructor = [].shift.call(arguments);

    obj.__proto__ = Constructor.prototype;

    var ret = Constructor.apply(obj, arguments);

    return typeof ret === 'object' ? ret : obj;

};
`