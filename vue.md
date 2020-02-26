### MVVM

Model–View–ViewModel (MVVM) 是一种软件架构设计模式

Model 层，模型层，后端传递的数据

View 层，视图层，前端展示

ViewModel 层，视图模型层，用于连接 View 层和 Model 层。一方面，实现了与 View 层的双向绑定，通过数据驱动视图，一方面，又与 Model 层进行交互，解耦了 View 层和 Model 层

 

双向绑定，实现数据视图自动更新，不再操作 DOM 对象，专注于业务逻辑

解耦了 View 层和 Model 层，目的在于实现前后端分离

&emsp;

### Vue 优势

1. 通过虚拟 DOM 减少 DOM 操作

   虚拟 DOM 采用 js 对象模拟一颗简单的 DOM 树，任何 DOM 操作都会在 VNode 上进行，最后新旧 VNode 进行对比（优化），一次性将比较结果更新到 DOM 树上。由于不需要频繁操作 DOM，大大提高了性能。

2. 双向数据绑定

   Vue 基于 MVVM 架构。在 MVVM 中， View 层和 Model 层不能直接通信，需要通过 ViewModel 层进行连接。当数据发生变化时，ViewModel 监听数据变化，更新视图；当用户操作视图时，ViewModel 监听视图变化，通知数据改动。Vue 实现双向数据绑定，可以让开发者不再操作 DOM 对象，专注于业务逻辑。

3. 组件化开发

   Vue 允许我们使用小型、独立和可复用的组件来构建应用。

   优点：组件高内聚低耦合，便于测试和复用

   ​			开发易于管理和协同，提高开发效率

> 发现两种双向数据绑定的理解：一种是 View 层和 ViewModel 层的绑定，一种是 View 层和 Model 层的绑定。其实，两个都有道理，前者适用于 Vue 框架，而后者基于整个 MVVM 架构。

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
   
3. el 选项提供一个在页面上已经存在的 DOM 元素作为 Vue 实例的挂载目标（挂载点），挂载的元素会被 Vue 生成的 DOM 替换。

4. `Watcher` 的原理是通过对“被观测目标”（expOrFn）的求值，触发数据属性的 `get` 拦截器函数从而收集依赖。

   如果 `expOrFn` 中包含多个数据属性，执行 `expOrFn` 时会触发这些数据属性的 `get` 拦截器函数将当前观察者收集为依赖，即**一个观察者对象对应多个数据属性**

   如果对某个数据属性进行观察（Vue 的 watch 选项或者 vm.$watch），实际上就是定义了 `Watcher` 对象，会触发该数据属性的 `get` 拦截器函数收集依赖，结合上面的情况，即**一个数据属性对应多个观察者对象**
   
5. 读取数据属性的值，能够触发该属性的 get 拦截函数，从而收集依赖。但是其子属性是没有被读取的，所以也不能够收集依赖，千万要和定义响应式区分开来。

6. 如果计算属性 `compA` 依赖了数据对象的 `a` 属性，那么属性 `a` 将收集计算属性 `compA` 的 **计算属性观察者对象**，而 **计算属性观察者对象** 将收集 **渲染函数观察者对象**，实际上是属性`a`收集了**渲染函数观察者对象**，所以改变属性`a`的值，会触发渲染函数重新执行。



### Vue SSR

##### 什么是服务端渲染（SSR）

Vue.js 是构建客户端应用程序的框架。默认情况下，可以在浏览器中输出 Vue 组件，进行生成 DOM 和操作 DOM。然而，也可以将同一个组件渲染为服务器端的 HTML 字符串，将它们直接发送到浏览器，最后将这些静态标记"激活"为客户端上完全可交互的应用程序。

服务器渲染的 Vue.js 应用程序也可以被认为是"同构"或"通用"，因为应用程序的大部分代码都可以在**服务器**和**客户端**上运行。



##### 为什么用服务端渲染

与传统 SPA (Single-Page Application) 相比，服务器端渲染的优势主要在于：

- 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
- 更快的内容到达时间 (time-to-content)，特别是对于缓慢的网络情况或运行缓慢的设备。无需等待所有的 JavaScript 都完成下载并执行，你的用户将会更快速地看到完整渲染的页面，可以产生更好的用户体验。



##### vue-hackernews-2.0

官方示例项目

![hn-architecture](assets/hn-architecture.png)

[客户端应用程序和服务器应用程序](https://wangfuda.github.io/2017/05/14/vue-hackernews-2.0-code-explain/)，都要使用 webpack 打包 ：服务器需要「server bundle」然后用于服务器端渲染(SSR)，而「client bundle」会发送到浏览器，用于混合静态标记。

> bundle 可理解为打包后的东西

服务端应用程序需要打包的原因：	

- 通常 Vue 应用程序是由 webpack 和 vue-loader 构建的，并且许多 webpack 特定功能不能直接在 Node.js 中运行（例如通过 `file-loader` 导入文件，通过 `css-loader` 导入 CSS）。
- 尽管 Node.js 最新版本能够完全支持 ES2015 特性，我们还是需要转译客户端代码以适应老版浏览器。这也会涉及到构建步骤。

此外，bundle renderer 还具有以下优点：

- 内置的 source map 支持（在 webpack 配置中使用 `devtool: 'source-map'`）
- 在开发环境甚至部署过程中热重载（通过读取更新后的 bundle，然后重新创建 renderer 实例）
- 关键 CSS(critical CSS) 注入（在使用 `*.vue` 文件时）：自动内联在渲染过程中用到的组件所需的CSS。
- 使用 [clientManifest](https://ssr.vuejs.org/zh/api/#clientmanifest) 进行资源注入：自动推断出最佳的预加载(preload)和预取(prefetch)指令，以及初始渲染所需的代码分割 chunk。



##### 详细过程

服务器端：当 node server 收到来自 browser 的请求后，会创建一个 Vue 渲染器 Bundle Renderer，它会读取 server bundle（可选读取 template 页面模版和 clientManifest 客户端构建清单）并执行。 server bundle 实现了数据预取并返回已填充数据的 Vue 实例，接下来 Vue 渲染器内部就会将 Vue 实例渲染进 html 模板，最后把这个完整的 html 发送到 browser。

Bundle Renderer 具有 client manifest 和 server bundle 之后，可以自动推断和注入资源预加载 / 数据预取指令(preload / prefetch directive)，以及 css 链接 / script 标签到所渲染的 html。

优点在于：

- 在生成的文件名中有哈希时，可以取代 html-webpack-plugin 来注入正确的资源 URL。
- 在通过 webpack 的按需代码分割特性渲染 bundle 时，我们可以确保对 chunk 进行最优化的资源预加载/数据预取，并且还可以将所需的异步 chunk 智能地注入为 \<script\> 标签，以避免客户端的瀑布式请求 (waterfall request)，以及改善可交互时间 (TTI - time-to-interactive)。

默认情况下，当提供 template 渲染选项时，资源注入是自动执行的。



客户端：当 browser 收到 html 后，开始加载 client bundle，然后以激活模式进行挂载（Vue 在浏览器端接管由服务端发送的静态 html，使其变为由 Vue 管理的动态 DOM）。

客户端激活原因：由于服务器已经渲染好了 html，显然无需将其丢弃再重新创建所有的 DOM 元素。相反，可以“激活"这些静态的 html，然后使他们成为动态的（能够响应后续的数据变化）。



### Nuxt.js

Nuxt.js 是一个基于 Vue.js 的通用应用框架。

通过对客户端/服务端基础架构的抽象组织，Nuxt.js 主要关注的是应用的 **UI渲染**。

我们的目标是创建一个灵活的应用框架，你可以基于它初始化新项目的基础结构代码，或者在已有 Node.js 项目中使用 Nuxt.js。

Nuxt.js 预设了利用 Vue.js 开发**服务端渲染**的应用所需要的各种配置。

我们还提供了一种命令叫：`nuxt generate` ，为基于 Vue.js 的应用提供生成对应的静态站点的功能。

作为框架，Nuxt.js 为 `客户端/服务端` 这种典型的应用架构模式提供了许多有用的特性，例如异步数据加载、中间件支持、布局支持等。

​	