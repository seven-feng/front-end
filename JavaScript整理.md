# JavaScript整理

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
      func.apply(this);
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

![throttle](C:\Users\Administrator\Desktop\throttle.png)

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



