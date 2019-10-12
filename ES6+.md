&emsp;

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

