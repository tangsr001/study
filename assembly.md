什么是组件化?
组件化是一种代码拆分,解耦的的编程方式.
使用组件
因此总结为:组件化就是把一个大的功能实现进行代码拆分,拆分实现之后,在实现整个业务时,在对小组件进行组合
如何拆分?
首先我们来看看目前组件应用有哪些?
基础组件,业务组件,区块组件,和页面组件
基础组件:是指 input,button 这些基础标签,以及antd el 封装过的通用ui组件
业务组件:由基础组件合成的某个功能的,服务端封装了相应的 class
区块组件:由基础组件合业务组件合成的ui快
页面组件:展示给用户的最终页面
那么组件怎么结合呢,我们可以根据组件的数据传递方式和组件的逻辑处理方式得到两种方式 渐进式和离散式
渐进式:当数据逐层传递,组件各自负责自己的业务逻辑,例如vue 提供了v-model 数据双向绑定机制,容易处理上下间的数据,缺点就是同级兄弟组件通信缺少灵活性
离散式: 数据统一处理,集中处理业务逻辑  例如react 提倡的是函数式编程,是不完全的双向数据绑定的 JavaScript 框架,通常需要onChange 事件来更新父级的state 来更新 View,因此更适合离散型组合

下面我们来实现一个组件
首先我们安装一下工具
` 
  yarn global add @vue/cli
  vue create vui
`
安装后目录:
https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/9/170bef95c27a9ae0~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp
我们对目录做一些调整
https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2020/3/9/170befc719123264~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp

我们将 src 重命名为 examples,并添加 packages 目录, 用来存放我们的自定义组件.cli 默认会启动 src 的服务,当我们修改脚手架的目录结构之后,我们需要对配置文件也做一些修改
具体对 vue.config.js 修改如下
`
module.exports ={
  pages:{
     index:{
       entry:'examples/main.js',
       template:'public/index.html',
       filename:'index.html'
     }
  },
  // 扩展 webpack 配置,使 packages 加入编译
  chainWebpack:config => {
    .rule('js')
    include
    .add('./packages')
    .end()
    .use('babel'),
    .loader('babel-loader')
  }
}
`
首先修改入口文件地址为 examples 下的main.js ,其次将packages 打包编译任务当中

接下编写组件
首先我们拿一个 Button 组件来示范，这里只实现一个比较简单的组件，我们在packages 目录下新建一个 Button 目录，然后src 里存在组件的源代码
`  
 <template>
   <div class="x-button">
     <slot></slot>
   </div>
 </template>
 <script>
export default {
  name: 'x-button',
  props: {
    type: String
  }
}
</script>

<style scoped>
  .x-button {
      display: inline-block;
      padding: 3px 6px;
      background: #000;
      color: #fff;
  }
</style>

`
vue 和 react组件设计会大量的应用插槽机制，比如vue 里的 slot 标签，react 的 children 等，所以这一块需要关注
接着在button 里面的index.js 编写 vue 的安装

`
// 导入组件，组件必须声明 name
import XButton from './src'

// 为组件提供 install 安装方法，供按需引入
XButton.install = function (Vue) {
  Vue.component(XButton.name, XButton)
}

// 导出组件
export default XButton

`
https://juejin.cn/post/6844904085808742407#heading-4

 

