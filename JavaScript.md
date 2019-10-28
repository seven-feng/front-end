### typeof 和 instanceof

基本类型：Undefined、Null、Boolean、Number、String、BigInt

引用类型：

1. 原生引用类型：Object、Array、Date、RegExp、Function等
2. 自定义引用类型



确定一个值是哪种基本类型可用 typeof 操作符

| 操作符 | 值                      | 类型      |
| ------ | ----------------------- | --------- |
| typeof | "abc"                   | string    |
|        | true                    | boolean   |
|        | 123                     | number    |
|        | 123n                    | bigint    |
|        |                         | undefined |
|        | null （对应 Null 类型） | object    |
|        | new Object()            | object    |
|        | function() {}           | function  |

> 从逻辑角度来看，null 值表示一个**空对象指针**，所以使用 typeof 操作符检测 null 值时会返回 "object"
>
> undefined 派生自 null，所以 undefined == null 的值是 true

&emsp;

确定一个值是哪种引用类型可用 instanceof 操作符

| 值   | 操作符     | 构造函数 | 结果 |
| ---- | ---------- | -------- | ---- |
| obj  | instanceof | Object   | true |

可以使用 instanceof 操作符来测试实例与原型链中出现过的构造函数

> 每个引用类型都有一个对应的构造函数，可通过 new 操作符来创建实例

&emsp;

### window 和 document

window 对象表示浏览器的一个实例，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象（因此所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法）

document 对象是 window 对象的一个属性，表示整个 HTML 页面

&emsp;

### 闭包

指有权访问另一个函数作用域中的变量的函数

&emsp;

### for-in 和 for-of

for-in 主要用于遍历对象

~~~javascript
var obj = {a: 1,b: 2};
for(var o in obj) {
  console.log(o);
}
// a
// b

// 遍历数组时，返回的是数组下标
var array  = [2,3,4,5];
for(var i in array) {
  console.log(i);
}
// 0 1 2 3
~~~

&emsp;

for-of 不仅支持数组，还支持大多数类数组对象，还包括字符串

~~~javascript
var array  = [2,3,4,5];
for(var i of array) {
  console.log(i);
}
// 2 3 4 5

var array  = "abc";
for(var i of array) {
  console.log(i);
}
// a b c
~~~

&emsp;

### 对象属性遍历

Object.keys() 返回一个由**对象本身**的可枚举属性组成的数组

for…in 遍历**对象本身**的可枚举属性，以及对象从其**原型**中继承的属性

obj.hasOwnProperty(propertyName) 用于检查给定的属性是否在当前对象实例中（而不是在实例的原型中）

> 在“深拷贝”中，可以用 Object.keys() 代替 for…in + obj.hasOwnProperty(propertyName)

&emsp;

### Object 构造函数的方法

Object.create() 方法创建一个新对象，使用现有的对象来提供新创建的对象的\_\_proto\_\_

Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

&emsp;

### Object 构造函数原型的方法

Object.prototype.isPrototypeOf() 方法用于测试一个对象是否存在于另一个对象的原型链上

Object.prototype.hasOwnProperty(prop) 方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性

&emsp;

### 组合使用构造函数模式与原型模式

构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性

``` javascript
function SuperType(name) {
    this.name = name;
}

SuperType.prototype.sayName = function() {
    console.log(this.name);
}

function SubType(name,age) {
    SuperType.call(this,name);
    this.age = age;
}

SubType.prototype = new SuperType();

SubType.prototype.sayAge = function() {
    console.log(this.age);
}

var sub = new SubType('zhangsan',29);
sub.sayAge();
sub.sayName();
```

&emsp;

### 深拷贝

``` javascript
function deepClone(obj) {
  if (obj == null) { // 增加对象或属性值为null的情况
    return null;
  }
  var clone = Array.isArray(obj)? [] : {};
  for(var key in obj) {
    if (obj.hasOwnProperty(key)) {
      if (typeof obj[key] == 'object') {
        clone[key] = deepClone(obj[key]);
      } else {
        clone[key] = obj[key];
      }
    }
  }
  return clone;
}
```

&emsp;

### 防抖、节流、懒加载

目的：防止频繁触发事件，增加浏览器负担，影响用户体验

#### 防抖

函数防抖：当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，如果设定的时间到来之前，又一次触发了事件，就重新开始延时。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发

``` javascript
function debounce(func, delay) {
  var timer = null;
  return function() {
    if(timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(func,delay);
  }
}
window.onscroll = debounce(lazyLoad,1000);
```

#### 节流

函数节流：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。原理是通过判断是否到达一定时间来触发函数

``` javascript
function throttle(func, delay) {
  var timer = null, pre = 0;
  return function() {
    var context = this;
    var cur = Date.now();
    var remain = delay - (cur - pre);
    clearTimeout(timer);
    if (remain <= 0) {
      func.apply(context);
      pre = Date.now();
    } else {
      timer = setTimeout(function() {
        func.apply(context);
        pre = Date.now();
      }, remain)
    }
  }
}
```

![throttle](assets/throttle.png)

#### 懒加载

先将img标签中的src链接设为同一张图片（空白图片），将其真正的图片地址存储在img标签的自定义属性中（比如data-src）。当js监听到该图片元素进入可视窗口时，即将自定义属性中的地址存储到src属性中，达到懒加载的效果

``` javascript
var imgs = document.querySelectorAll('img');
var n = 0;

function lazyLoad() {
  var viewHight = document.body.clientHeight;
  var scrollTop =  document.body.scrollTop;
  
  for(var i = n; i < imgs.length; i++) {
    if (imgs[i].offsetTop <= viewHight + scrollTop ) {
      if (imgs[i].getAttribute('src') == '') {
        imgs[i].setAttribute('src', imgs[i].getAttribute('data-src'));
      }
      n = i + 1;
    }
  }
}

window.onload = lazyLoad;
window.onscroll = throttle(lazyLoad, 1000);
```

&emsp;

### bind函数

``` javascript
Function.prototype.bind = function( context ) {
  var self = this; // 保存原函数
  return function(){ // 返回一个新的函数
  	return self.apply( context, arguments ); 
  	// 执行新的函数的时候，会把之前传入的context当作新函数体内的this
  }
}

var obj = { name: 'seven' };

var func = function() { console.log( this.name ); }.bind(obj)

func(); // 输出：seven

/*
*	function() { console.log( this.name ); } 是 Function 类型的实例，可以调用 bind 函数
*	绑定 obj 对象， 返回一个新函数
*/

Function.prototype.bind = function() {
  var self = this;
  var context = [].shift.apply(arguments);
  var args = [].slice.apply(arguments);
  return function() {
    return self.apply( context, [].concat(args, [].slice.apply(arguments)) );
  }
}

var obj = { name: 'seven' };

var func = function( a, b, c, d ) {
  console.log( this.name ); // 输出：seven
  console.log( [ a, b, c, d ] ) // 输出：[ 1, 2, 3, 4 ]
}.bind(obj,1,2);

func(3,4);
```

&emsp;

### 跨浏览器事件处理程序

``` javascript
var addEvent = function(elem, type, handler) {
    if(elem.addEventListener) {
        elem.addEventListener(type, handler, false);
    } else if(elem.attachEvent) {
        elem.attachEvent('on'+type, handler);
    } else {
        elem['on'+type] = handler;
    }
}

var btn = document.getElementById("btn");
addEvent(btn, "click", function() {
    alert("success!");
});
```

&emsp;

### 事件处理程序

~~~javascript
// HTML事件处理程序
<input type="button" value="Click Me" onclick="alert('Clicked')" />

// DOM0级事件处理程序
var btn = document.getElementById("myBtn");
btn.onclick = function() {
	alert("Clicked");
};

// DOM2级事件处理程序
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function() {
	alert(this.id);
}, false);
// 可添加多个，添加顺序触发
// true表示捕获阶段，false表示冒泡阶段

// IE事件处理程序
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function() {
alert("Clicked");
});
// 可添加多个，添加相反顺序触发

~~~

&emsp;

### JS 正则表达式

| 元字符 | 描述                       |
| ------ | -------------------------- |
| /      | 表示正则表达式的开始和结束 |
| ^      | 匹配字符串的开始           |
| $      | 匹配字符串的结束           |
| \d     | 匹配一个数字字符           |

&emsp;

### JS 异步编程

- 回调函数
- 事件监听
- 发布/订阅
- Promise 对象

&emsp;

### JS 单线程和异步

js 是单线程的，浏览器只分配给 js 一个主线程，用来执行任务（函数），一次只能执行一个。

浏览器（js的宿主环境）是多线程的，可以为异步任务开辟单独的线程，异步任务完成之后，将回调函数放入任务队列，所以说浏览器是事件驱动的，通过执行一个个事件来运行。

整个执行过程：js 主线程执行一个任务，推入执行栈，执行函数时，如有其他函数，就继续入栈，执行完就出栈。等到执行栈为空时，就从任务栈中读取新任务入栈，读取任务的过程就叫作 event loop。

&emsp;

### Event

e.target：触发事件的元素

e.currentTarget：绑定事件的元素

e.offsetX：当鼠标事件发生时，鼠标相对于事件源 x 轴的位置

e.offsetY：当鼠标事件发生时，鼠标相对于事件源 y 轴的位置

e.clientX：当鼠标事件发生时，鼠标相对于浏览器（这里说的是浏览器的有效区域）x轴的位置

e.clientY：当鼠标事件发生时，鼠标相对于浏览器（这里说的是浏览器的有效区域）y轴的位置

e.pageX：e.clientX + 横向滚动条距离

e.pageY：e.clientY + 纵向滚动条距离

e.screenX：当鼠标事件发生时，鼠标相对于显示器屏幕 x 轴的位置

e.screenY：当鼠标事件发生时，鼠标相对于显示器屏幕 y 轴的位置

&emsp;

### Dom 元素

HTMLElement.offsetParent，指向最近的包含该元素的定位元素

HTMLElement.offsetLeft，返回当前元素相对于其 offsetParent 元素的左侧的距离

HTMLElement.offsetTop，返回当前元素相对于其 offsetParent 元素的顶部的距离

HTMLElement.offsetWidth，返回元素的布局宽度

HTMLElement.offsetHeight，返回元素的布局高度

![HTMLElement-offsetwidth-offsetheight](assets/HTMLElement-offsetwidth-offsetheight.png)

Element.scrollLeft，表示元素横向滚动的距离

Element.scrollTop，表示元素纵向滚动的距离

Element.scrollWidth，表示元素滚动视图的宽度（包括由于溢出导致的视图中不可见内容 ）

Element.scrollHeight，表示元素滚动视图的高度（包括由于溢出导致的视图中不可见内容 ）

Element.clientWidth，表示元素内部的宽度

Element.clientHeight，表示元素内部的高度

![element-clientwidth-clientheight](assets/element-clientwidth-clientheight.png)



