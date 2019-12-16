### MVVM

Model–View–ViewModel (MVVM) 是一种软件架构设计模式

Model 层，模型层，后端传递的数据

View 层，视图层，前端展示

ViewModel 层，视图模型层，用于连接 View 层和 Model 层。一方面，实现了与 View 层的双向绑定，通过数据驱动视图，一方面，又与 Model 层进行交互，解耦了View层和Model层

 

双向绑定，实现数据视图自动更新，减少 DOM 操作，专注于业务逻辑

解耦了 View 层和 Model 层，目的在于实现前后端分离

&emsp;

### created 和 mounted 的区别

#### created

在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图

#### mounted

在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作

&emsp;

### 组件的生命周期

#### 父子组件

![vue-lifecycle1](assets/vue-lifecycle1.png)

#### 继承组件

![vue-lifecycle2](assets/vue-lifecycle2.png)

&emsp;

### vue 原理

vue.js 采用数据劫持结合发布-订阅模式的方式，通过 Object.defineProperty() 来劫持各个属性的setter、getter，在数据变动时发布消息给订阅者，触发相应的监听回调

> 属性描述符：数据描述符和存取描述符

&emsp;

### 虚拟DOM

用 js 对象模拟 DOM 节点，用户交互时操作虚拟 DOM，然后通过 diff 算法比较操作前后的虚拟 DOM，最后将 diff 结果更新到实际 DOM 上

&emsp;

### vuex 缺陷

没有持久化存储的手段，每次刷新都会重置所有的数据

&emsp;

看 vue 源码心得：

1. 数据对象是数组时，数组的索引是非响应式的

   ~~~js
   vm = new Vue({
     data: {
       arr: [1, 2]
     }
   })
   
   vm.arr[0] = 3  // 不能触发响应
   vm.arr.push(4) // 出发响应
   ~~~

   对数组的处理，都是通过拦截数组的变异方法实现的
   
2. 数组的索引是非响应式的原因

   Vue 中通过对对象每个键设置 getter 和 setter 来实现响应式。使用数组的目的往往是遍历，但是调用 getter 开销太大，所以 Vue 不在数组的每个键上定义响应式。