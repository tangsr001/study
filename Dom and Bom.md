DOM: 文档对象模型,提供操作页面元素的方法和属性
BOM: 浏览器对象模型,提供以属性和方法操作浏览器
javascript 由三部分组成 ECMAScript ,DOM 和 BOM 
ECMAScript 描述了JS的语法和基本接口
DOM 是文档对象,处理网页内容的方法和接口
BOM 是浏览器模型,提供与浏览器交互的方法和接口
DOM 全称是Document Object Model ,也就是文档对象模型,是针对 XML 的基于树的 API 描述了处理网页内容的方法和接口,是HTML 和 XML 的API,DOM 把整个页面规划右节点层级狗层的文档
这个DOM 定义了一个HTMLDocument 和 HTMLElement 做这种实现的基础,可以对这个HTML 的内容继续操作,删除,添加,修改,查找
BOM 是 brower Object Modl 浏览器对象模型,其主要的功能是页面的加载,滚动,跳转,历史记录
BOM 的核心是Window 而Window 对象又具有双重角色,它既是通过 js 访问浏览器窗口的一个接口,又是一个GLOBAL 全局对象,这意味着在网页中定义的任何对象,函数
