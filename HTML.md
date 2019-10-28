### defer 和 async

\<script\> 标签中有 defer 或 async 属性，脚本就会异步加载。渲染引擎遇到这一行命令，就会开始下载外部脚本，但不会等它下载和执行，而是直接执行后面的命令。

#### 区别

defer 要等到整个页面在内存中正常渲染结束（DOM 结构完全生成，以及其他脚本执行完成），才会执行。async 一旦下载完，渲染引擎就会中断渲染，执行这个脚本以后，再继续渲染。

一句话，defer 是“渲染完再执行”，async 是“下载完就执行”。另外，如果有多个 defer 脚本，会按照它们在页面出现的顺序加载，而多个 async 脚本是不能保证加载顺序的

&emsp;

### HTML 标签

```html
<dl>定义列表
<dt>术语
<dd>描述
```

&emsp;

### 拖放

拖放是 HTML5 标准的组成部分，任何元素都是可拖放的。 

&emsp;

##### 确定什么是可拖动的 

让一个元素被拖动需要添加 draggable 属性，再加上全局事件处理函数 ondragstart

~~~ html
<script>
function dragstart_handler(ev) {
 // Add the target element's id to the data transfer object
 ev.dataTransfer.setData("text/plain", ev.target.innerText);
}
</script>

<p id="p1" draggable="true" ondragstart="dragstart_handler(event)">This element is draggable.</p>
~~~

&emsp;

##### 定义拖动数据

应用程序可以在拖动操作中包含任意数量的数据项。每个数据项都是一个 string 类型，典型的 MIME 类型，如：text/html。 

每个 drag event 都有一个 dataTransfer 属性保保存事件的数据。这个属性（ DataTransfer对象）也有管理拖动数据的方法，setData() 方法添加一个项目的拖拽数据。 

~~~ javascript
function dragstart_handler(ev) {
  // 添加拖拽数据
  ev.dataTransfer.setData("text/plain", ev.target.innerText);
  ev.dataTransfer.setData("text/html", ev.target.outerHTML);
}
~~~

&emsp;

##### 定义拖动图像

拖动过程中，浏览器会在鼠标旁显示一张默认图片。当然，应用程序也可以通过 setDragImage() 方法自定义一张图片。

~~~ javascript
function dragstart_handler(ev) {
  // Create an image and then use it for the drag image.
  // NOTE: change "example.gif" to a real image URL or the image 
  // will not be created and the default drag image will be used.
  var img = new Image(); 
  img.src = 'example.gif'; 
  ev.dataTransfer.setDragImage(img, 10, 10);
}
~~~

&emsp;

#####  定义拖动效果

dropEffect 属性用来控制拖放操作中用户给予的反馈。它会影响到拖动过程中浏览器显示的鼠标样式。比如，当用户悬停在目标元素上的时候，浏览器鼠标也许要反映拖放操作的类型。

有 3 个效果可以定义： 

1. `copy` 表明被拖动的数据将从它原本的位置拷贝到目标的位置。
2. `move` 表明被拖动的数据将被移动。
3. `link` 表明在拖动源位置和目标位置之间将会创建一些关系表格或是连接。

在拖动过程中，拖动效果也许会被修改以用于表明在具体位置上具体效果是否被允许，如果允许，在该位置则被允许放置。

~~~ javascript
function dragstart_handler(ev) {
  ev.dataTransfer.dropEffect = "copy";
}
~~~

&emsp;

##### 定义一个放置区 

当拖动一个项目到 HTML 元素中时，浏览器默认不会有任何响应。想要让一个元素变成可释放区域，该元素必须设置 ondragover 和 ondrop 事件处理程序属性。

~~~html
<script>
function dragover_handler(ev) {
 ev.preventDefault();
 ev.dataTransfer.dropEffect = "move";
}
function drop_handler(ev) {
 ev.preventDefault();
 // Get the id of the target and add the moved element to the target's DOM
 var data = ev.dataTransfer.getData("text/plain");
 ev.target.appendChild(document.getElementById(data));
}
</script>

<p id="target" ondrop="drop_handler(event)" ondragover="dragover_handler(event)">Drop Zone</p>
~~~

调用 ev.preventDefault() 来阻止数据的浏览器默认处理方式（drop 事件的默认行为是以链接形式打开）

 

#####  处理放置效果

drop 事件的处理程序是以程序指定的方法处理拖动数据。一般，程序调用 getData() 方法取出拖动项目并按一定方式处理。程序意义根据 dropEffect 的值与/或可变更关键字的状态而不同 

~~~ html
<script>
function dragstart_handler(ev) {
 // Add the target element's id to the data transfer object
 ev.dataTransfer.setData("application/my-app", ev.target.id);
 ev.dataTransfer.dropEffect = "move";
}
function dragover_handler(ev) {
 ev.preventDefault();
 ev.dataTransfer.dropEffect = "move"
}
function drop_handler(ev) {
 ev.preventDefault();
 // Get the id of the target and add the moved element to the target's DOM
 var data = ev.dataTransfer.getData("application/my-app");
 ev.target.appendChild(document.getElementById(data));
}
</script>

<p id="p1" draggable="true" ondragstart="dragstart_handler(event)">This element is draggable.</p>
<div id="target" ondrop="drop_handler(event)" ondragover="dragover_handler(event)">Drop Zone</div>
~~~

&emsp;

#####  拖动结束

拖动操作结束时，在源元素（开始拖动时的目标元素）上触发 dragend 事件。不管拖动是完成还是被取消这个事件都会被触发。 dragend 事件处理程序可以检查 dropEffect 属性的值来确认拖动成功与否。 