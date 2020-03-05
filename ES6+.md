### ES 声明变量方法

var、function、let、const、import、class

&emsp;

### var、let、const

|       | 声明类型 | 跨块访问 | 跨函数访问 |                |
| ----- | -------- | -------- | ---------- | -------------- |
| var   | 变量     | 可以     | 不可以     |                |
| let   | 变量     | 不可以   | 不可以     | 先声明、后使用 |
| const | 常量     | 不可以   | 不可以     | 先声明、后使用 |

> for 循环的计数器，就很合适使用 let 命令
>
> for 循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域

&emsp;

### Class 定义

~~~javascript
class Point {
	constructor(x, y) {
  		this.x = x;
  		this.y = y;
 	}
 	toString() {
  		return '(' + this.x + ', ' + this.y + ')';
 	}
}
~~~

&emsp;

### 解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

常见的有数组的解构赋值，对象的结构赋值

#### 用途

1. 交换变量的值
2. 从函数返回多个值
3. 函数参数的定义
4. 提取 JSON 数据

&emsp;

### 箭头函数

箭头函数没有 this 绑定，意味着箭头函数内部的 this 值只能通过查找作用域链来确定。如果箭头函数被包含在一个非箭头函数内，那么 this 值就会与该函数的相等。否则，this 值就会是全局对象（在浏览器中是 window，在nodejs 中是 global）

&emsp;

非箭头函数内部的 this 值指向调用它的对象

 ~~~javascript
var a = {
	b: () => { console.log(this); },
	c: function() { console.log(this); }
}

a.b(); // window
a.c(); // a
 ~~~

&emsp;

~~~javascript
var a = 11;
function test2() {
	this.a = 22;
	let b = ()=> { console.log(this.a); }
 	b();
}

var x = new test2();
// 输出22
~~~

由于箭头函数的 this 值由包含它的函数决定，因此不能使用 call()、apply()、bind() 方法来改变其 this 值

&emsp;

### ES6 模块化

1. 一个模块就是一个独立的文件
2. 一个模块只加载一次， 且只执行一次， 如果再次加载同目录下同文件，可直接从内存中读取。 一个模块就是一个单例，或者说就是一个对象
3. 一个模块内声明的变量都是局部变量， 不会污染全局作用域 
4. 一个模块内部的变量或者函数可以通过 export 导出 
5. 一个模块可以导入其他模块

&emsp;

ES6 模块的设计思想是尽量的静态化，使得编译时就能确定模块间的依赖关系，以及输入和输出的变量。

JS 引擎对脚本静态分析的时候，遇到模块加载命令import（编译时加载或静态加载），就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

总结一下，就是 ES6 模块在“编译时”加载，在“运行时”执行，而 CommonsJS 模块是在“运行时”加载并执行。 

#### 栗子

~~~javascript
// main.js
import {a} from './my'
console.log('222222');
export var b = 'bbbbbb';
console.log(a);

// my.js
import {b} from './main'
console.log('111111');
export var a = 'aaaaaaa';
console.log(b);

// 执行结果
111111
undefined
222222
aaaaaa
~~~

当执行 main.js 的时候，第一句是 import {a} from './my'，所以先执行my.js。虽然 my.js 的第一句是 import {b} from './main'，但是不会再执行 main.js，因为 main.js 已经在执行了。当 my.js 执行完后，继续执行 main.js 的console.log('222222');。  

&emsp;

> import 语句会执行所加载的模块，如果多次重复执行同一句 import 语句，那么只会执行一次
>
> import() 函数，完成动态加载，返回一个 Promise 对象

&emsp;

#### ES6 模块与 CommonJS 模块差异

- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用
- CommonJS 模块是运行时加载，ES6 模块是编译时输出接口

&emsp;

第二个差异是因为 CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。而 ES6 模块不是对象（指的是编译时加载阶段），而是通过 export 命令显式指定输出的代码，再通过 import 命令输入（编译时加载或静态加载），它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

&emsp;

我的理解：在编译阶段，main.js 的 import {a} 指向了 my.js 中 export 的 a，这时，my.js 只是静态加载（放入内存），还没有执行（代码中是一个赋值语句），所以 export 的 a 是 undefined，两者只是一个引用关系。等到了运行阶段，my.js 才真正被执行。

&emsp;

#### 循环加载的区别

CommonJS 的一个模块，就是一个 js 文件。require 命令第一次加载该脚本（运行时加载或动态加载），就会执行整个脚本，然后在内存生成一个对象，一旦出现某个模块被"循环加载"，就只输出已经执行的部分，还未执行的部分不会输出。

ES6 模块是动态引用（也就是说，真正执行时，才根据引用去取值），如果使用 import 从一个模块加载变量，该变量不会被缓存，而是成为一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值。

总结一下就是：编译阶段生成了引用，到了运行阶段才真正取值，所以说，只要引用存在，就可以执行。极端栗子就是，一个引用是 undefined，因为还没执行到，但是也不会报错，因为引用已经存在了。所以，两者的区别在于 CommonJS 模块的未执行部分不可以引用，会报错，但是ES6模块不会。

CommonJS模块是动态加载，执行了会输出，未执行就不会输出。ES6模块是静态加载，在编译阶段生成只读引用，如果未执行，引用就是undefined（参考上面的栗子）。

&emsp;

### Promise

Promise 对象的初始化接收一个执行函数 executor，executor 是带有 resolve 和 reject 两个参数的函数。

Promise 构造函数执行时会立即调用 executor 函数（executor 函数在 Promise 构造函数返回新建对象前被调用）。

resolve 和 reject 函数被调用时，分别将 promise 的状态改为 fulfilled（完成） 或 rejected（失败）。executor 内部通常会执行一些异步操作，一旦完成，可以调用 resolve 函数来将 promise 状态改成 fulfilled，或者在发生错误时将它的状态改为 rejected。

所以说，新生成的 Promise 对象可能有三种状态：peding（进行中）、fulfilled（已成功） 和 rejected（已失败）。如果 Promise 对象的状态变成 fulfilled，就会执行 then 的第一个回调函数，如果变成 rejected ，则会执行 then 的第二个回调函数（如果有的话）或者 catch 中的回调函数。

then 是链式的，可以返回一个 Promise 对象，也可以返回一个值，都会被下一个then的回调函数接收。

&emsp;

### Set

无重复值的有序列表

~~~ js
// 创建 Set
let set = new Set();
// 添加值
set.add(5);
set.add("5");
// 返回值的个数
console.log(set.size); // 2
// 检测值是否存在
console.log(set.has(5)); // true
console.log(set.has(6)); // false
// 移除值
set.delete(5);
// 移除所有值
set.clear();

// Set 构造器可以接收任意可迭代对象作为参数
let set = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
console.log(set.size); // 5

// forEach 方法，value 与 key 相同, ownerSet 是目标 Set 自身
let set = new Set([1, 2]);
set.forEach(function(value, key, ownerSet) {
	console.log(key + " " + value);
	console.log(ownerSet === set);
});

// 将 Set 转换为数组
let set = new Set([1, 2, 3, 3, 3, 4, 5]);
let array = [...set];
console.log(array); // [1,2,3,4,5]
~~~

### Weak Set

该类型只允许存储对象**弱引用**，而不能存储基本类型的值。对象的弱引用在它自己成为该对象的唯一引用时，不会阻止垃圾回收。

~~~ js
let set = new WeakSet(), key = {};
// 将对象加入 set
set.add(key);
console.log(set.has(key)); // true
// 移除对于键的最后一个强引用，同时从 Weak Set 中移除
key = null;
~~~

### Set 和 Weak Set 区别

1. 对于 WeakSet 的实例，若调用 add() 方法时传入了非对象的参数，就会抛出错误（has() 或 delete() 则会在传入了非对象的参数时返回 false ）
2. Weak Set 不可迭代，因此不能被用在 for-of 循环中
3. Weak Set 无法暴露出任何迭代器（例如 keys() 与 values() 方法），因此没有任何编程手段可用于判断 Weak Set 的内容
4. Weak Set 没有 forEach() 方法
5. Weak Set 没有 size 属性

&emsp;

### Map

键值对的有序列表，键和值都可以是任意类型，键的比较使用的是 Object.is()

~~~ js
// 创建 Map
let map = new Map();
// 添加项
map.set("name", "Nicholas");
map.set("age", 25);
// 返回键值对个数
console.log(map.size); // 2
// 判断指定的键是否存在于 Map 中
console.log(map.has("name")); // true
console.log(map.get("name")); // "Nicholas"
// 移除 Map 中的键以及对应的值
map.delete("name");
// 移除 Map 中所有的键与值
map.clear();

// Map 初始化
// 将数组传递给 Map 构造器，以便使用数据来初始化一个 Map 。该数组中的每一项也必须是数组，内部数组的首个项会作为键，第二项则为对应值
let map = new Map([["name", "Nicholas"], ["age", 25]]);
// forEach 方法
map.forEach(function(value, key, ownerMap) {
	console.log(key + " " + value);
	console.log(ownerMap === map);
});
~~~

### Weak Map

Weak 版本都是存储对象弱引用的方式。在 Weak Map 中，所有的**键**都必须是对象（尝试使用非对象的键会抛出错误），而且这些对象都是弱引用，不会干扰垃圾回收。当 Weak Map 中的**键**在 Weak Map 之外不存在引用时，该键值对会被移除。

WeakMap 类型是键值对的无序列表，其中键必须是非空的对象，值则允许是任意类型。

~~~js
let map = new WeakMap(), element = document.querySelector(".element");
map.set(element, "Original");
let value = map.get(element);
console.log(value); // "Original"
// 移除元素
element.parentNode.removeChild(element);
element = null;
// 该 Weak Map 在此处为空

// Weak Map 只有两个附加方法 has() 和 delete()，能用来与键值对交互
let map = new WeakMap(), element = document.querySelector(".element");
map.set(element, "Original");
console.log(map.has(element)); // true
console.log(map.get(element)); // "Original"
map.delete(element);
console.log(map.has(element)); // false
console.log(map.get(element)); // undefined
~~~

&emsp;

### async 函数

ES2017 标准引入了 async 函数，使得异步操作变得更加方便。

async 函数是什么？一句话，它就是 Generator 函数的语法糖。



##### 基本用法

`async`  函数返回一个 Promise 对象，可以使用 `then` 方法添加回调函数。当函数执行的时候，一旦遇到 `await` 就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

~~~js
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...)

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
~~~

&emsp;

##### 语法

`async` 函数返回一个 Promise 对象。

`async` 函数内部 `return` 语句返回的值，会成为 `then` 方法回调函数的参数。

`async` 函数内部抛出的错误对象会被 `catch` 方法回调函数接收到。

`async` 函数返回的 Promise 对象，必须等到内部所有 `await` 命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到 `return` 语句或者抛出错误。

&emsp;

`await` 命令后面如果是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。

用 `await` 实现一个简单的休眠

~~~js
function sleep(interval) {
  return new Promise(resolve => {
    setTimeout(resolve, interval);
  })
}

// 用法
async function one2FiveInAsync() {
  for(let i = 1; i <= 5; i++) {
    console.log(i);
    await sleep(1000);
  }
}

one2FiveInAsync();
~~~

`await` 命令后面的 Promise 对象如果变为 `reject` 状态，则 `reject` 的参数会被 `catch` 方法的回调函数接收到。

任何一个 `await` 语句后面的 Promise 对象变为 `reject` 状态，那么整个`async` 函数都会中断执行。

&emsp;

##### 注意点

- `await` 命令后面的 `Promise` 对象，运行结果可能是 `rejected`，所以最好把`await` 命令放在 `try...catch` 代码块中。
- 多个 `await` 命令后面的异步操作，如果不存在继发关系，最好让它们同时触发。
- `await` 命令只能用在 `async` 函数之中，如果用在普通函数，就会报错。
- `async` 函数可以保留运行堆栈。



##### async 函数的实现原理

async 函数的实现原理，就是将 Generator 函数和自动执行器，包装在一个函数里。

~~~js
async function fn(args) {
  // ...
}

// 等同于

function fn(args) {
  return spawn(function* () {
    // ...
  });
}
~~~

所有的 `async` 函数都可以写成上面的第二种形式，其中的 `spawn` 函数就是自动执行器。

~~~js
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
~~~

