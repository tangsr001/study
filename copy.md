深拷贝的实现方式
方法以一
先把一个对象或者是一个数组转化成字符串，然后再变成对象或者数组
`
  JSON.parse(JSON.stringify(obj))
`

方法二：递归实现，判断属性是不是一个object 类型，如果是，对这个属性进行循环遍历，如果是基本类型直接赋值
实现方法
` 
      const obj = {
        num: 3,
        str: "啦啦啦",
        boy: {
          height: 21,
          weight: 105,
        },
        arr: [1, 2, 3],
        fn: function () {
          console.log(2333);
        },
        reg: /"/g,
        date: new Date(),
      };

      function deepClone(obj) {
        let objClone = Array.isArray(obj) 1 [] : {};
        //如果是对象,则递归对其属性进行深拷贝
        if (obj && typeof obj === "object") {
          //取出键
          for (let key in obj) {
            //如果是对象并且是这个对象的属性
            if (obj[key] && typeof obj[key] === "object") {
                   newObj[i] =
                Object.prototype.toString.call(oldObj[i]) === "[object Array]"
                  ? []
                  : {};
              deepClone(newObj[i], oldObj[i]);
            } else {
              //如果不是，简单复制
              objClone[key] = obj[key];
            }
          }
        }
        return objClone;
      }

      const obj2 = deepClone(obj);
      console.log(obj2);

`

方法三：Object.create(obj)
`
  var demo = {
      b:1,
      c:2,
      d:'tom'
      e:function(){
          console.log(1)
      }
  }
  var cloneObj = Object.create(demo)
`
方法四：jQuery $.extend()
