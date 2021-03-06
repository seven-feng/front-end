### 盒模型

所有 HTML 元素都可以看作盒子

#### 标准盒模型

box-sizing: content-box 

element width（元素实际宽度） = width + border + padding

#### IE 盒模型

box-sizing: border-box

element width （元素实际宽度） = width

&emsp;

### css优先级

!important > 行内样式 > ID 选择器 > 类选择器 > 标签选择器 > 通配符 > 继承

行内样式：1,0,0,0

ID选择器：0,1,0,0

类选择器、属性选择器或伪类：0,0,1,0

元素和伪元素：0,0,0,1

&emsp;

 !important优先级最高

权重相同时，后面的样式优先级高

&emsp;

> 伪选择器专门用来选择特殊区域或者特殊状态下的元素或者对象，这些特殊区域或者特殊状态是无法通过标签选择器，ID选择器或者类选择器进行精确控制的

&emsp;

### 块级元素和行内元素

块级元素：div、p、h1~h6、ul、ol、dl、li、dd、table、hr、blockquote、address、table、menu、pre，HTML5新增的header、section、aside、footer等

行内元素：span、img、a、lable、input、abbr（缩写）、em（强调）、big、cite（引用）、i（斜体）、q（短引用）、textarea、select、small、sub、sup，strong、u（下划线）、button

#### 区别

块级元素会独占一行，默认情况下，其宽度自动填满其父元素宽度

行内元素不会独占一行，相邻的行内元素会排列在同一行里，直到一行排不下，才会换行，其宽度随元素的内容而变化

&emsp;

块级元素width、height、margin的四个方向、 padding的四个方向都正常显示，遵循标准的css盒模型，例如：div

**行内替换元素** width、height、margin 的四个方向、 padding 的四个方向都正常显示，遵循标准的 css 盒模型，例如：img

**行内非替换元素** width、height 不起作用，padding 左右起作用，上下不会影响行高，但是对于有背景色和内边距的行内非替换元素，背景可以向元素上下延伸，但是行高没有改变，margin 左右作用起作用，上下不起作用

&emsp;

### position 与 float 同时使用的情况

无论 position 和 float 谁写在前面或后面，都是 position 先起作用。

1. 当 position 为 absolute/fixed 时，元素已脱离文档流，再对元素应用 float 失效
2. 当 position 为 static/relative 时，元素依旧处于普通流中，再对元素应用 float 起作用

&emsp;

### 浮动元素

1. 浮动元素自动变成块级元素
2. 浮动元素会对非浮动元素的布局产生影响

&emsp;

#### 为什么要清除浮动

浮动元素脱离文档流，父元素的高度无法自适应

比如父元素的高度小于浮动元素的时候，此时就会出现“高度塌陷”

&emsp;

### rem

rem (font size of the root element) 指相对于HTML根元素的字体大小的单位

#### 自适应 rem 值

1. 使用媒体查询控制不同分辨率下的根元素字体大小
2. 通过JavaScript动态获取屏幕的宽度来设置根元素的字体大小

&emsp;

### Flex 布局

Flex（Flexible Box）意为弹性布局

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称”容器”。它的所有子元素自动成为容器成员，称为Flex 项目（flex item），简称”项目”

~~~ css
.box{
  display: flex;
}
~~~



容器属性：flex-direction、flex-wrap、flex-flow、justify-content、align-items、align-content

项目属性：order、flex-grow、flex-shrink、flex-basis、flex、align-self

 &emsp;

flex 属性是 flex-grow, flex-shrink 和 flex-basis 的简写

`flex: initial`

元素会根据自身宽高设置尺寸。它会缩短自身以适应 flex 容器，但不会伸长并吸收 flex 容器中的额外自由空间来适应 flex 容器 。相当于将属性设置为 `flex: 0 1 auto`。

`flex: auto`

元素会根据自身的宽度与高度来确定尺寸，但是会伸长并吸收 flex 容器中额外的自由空间，也会缩短自身来适应 flex 容器。这相当于将属性设置为 `flex: 1 1 auto`。

`flex: none`

元素会根据自身宽高来设置尺寸。它是完全非弹性的：既不会缩短，也不会伸长来适应 flex 容器。相当于将属性设置为 `flex: 0 0 auto`。

```css
/* One value, unitless number: flex-grow */
flex: 2;

/* One value, width/height: flex-basis */
flex: 30px;

/* Two values: flex-grow | flex-basis */
flex: 1 30px;

/* Two values: flex-grow | flex-shrink */
flex: 2 2;

/* Three values: flex-grow | flex-shrink | flex-basis */
flex: 2 2 10%;
```

&emsp;

### CSS 预处理器

less、sass、scss

CSS 预处理器用一种专门的编程语言，进行Web页面样式的设计，然后再编译成正常的 CSS 文件，以供项目使用。

> 我的理解就是：css 预处理器可以让你用编程的方式来设计样式，也就是说为 css 加入了编程的特性

&emsp;

### translate(50%, 50%)

translate(x, y) 的百分比数据，会以**本身的长宽**做参考。比如，本身的长为100px，高为100px。那 (50%, 50%) 就是向右，向下移动50px，添加负号就是向着相反的方向移动。

