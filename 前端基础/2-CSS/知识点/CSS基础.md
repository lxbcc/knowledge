参考文档
[1.5 万字 CSS 基础拾遗（核心知识、常见需求）本篇文章围绕了 CSS 的核心知识点和项目中常见的需求来展开。虽然行 - 掘金](https://juejin.cn/post/6941206439624966152?searchId=2024102019385247323C14687640133F98#heading-2)
## CSS语法

选择器和生命块组成了CSS规则集

```css
span {
    color: red;
    text-align: center;
}

/* 单行注释 */

/*
    多行
    注释
*/
```

## @规则

CSS 规则是样式表的主体，通常样式表会包括大量的规则列表。但有时候也需要在样式表中包括其他的一些信息，比如字符集，导入其它的外部样式表，字体等，这些需要专门的语句表示。

@规则包含：
- `@namespace` 是用来定义使用在 CSS 样式表中的 XML 命名空间的 @规则
- `@media` 如果满足媒体查询的条件则条件规则组里的规则生效。
- `@page` 描述打印文档时布局的变化
- `@font-face` 指定一个用于显示文本的自定义字体
- `@keyframes` 描述 CSS 动画的关键帧
- `@layer`
- `@import `用于告诉 CSS 引擎引入一个外部样式表

  link和import的区别

## CSS特性

### 层叠性

解决样式冲突的问题
比如说针对某个 HTML 标签，有许多的 CSS 声明都能作用到的时候，那最后谁应该起作用呢？
   
针对不同源的样式，将按照如下的顺序进行层叠，越往下优先级越高：
- 用户代理样式表中的声明(例如，浏览器的默认样式，在没有设置其他样式时使用)。
- 作者样式表中的常规声明(这些是我们 Web 开发人员设置的样式)。
- 作者样式表中的 !important 声明。

需结合优先级以及继承性来理解
   
### 优先级:

   ![[Pasted image 20241020204738.png|500]]
#### 权重
   
 10000：!important；
 01000：内联样式；
 00100：ID 选择器；
 00010：类选择器、伪类选择器、属性选择器；
 00001：元素选择器、伪元素选择器；
 00000：通配选择器( * )、后代选择器(>)、兄弟选择器(+)；
 
 样式表的来源不同时，优先级顺序为：内联样式 > 内部样式 > 外部样式 > 浏览器用户自定义样式 > 浏览器默认样式。
   
#### 注意事项
一定要优先考虑使用样式规则的优先级来解决问题而不是 !important；
只有在需要覆盖全站或外部 CSS 的特定页面中使用 !important；
永远不要在你的插件中使用 !important；
永远不要在全站范围的 CSS 代码中使用 !important；
### 继承性

子元素会继承父元素对应属性计算后的值
一定是那些不会影响到页面布局的属性，存在默认继承的行为
   
<mark style="background: #BBFABBA6;">字体相关</mark>：`font-family`、`font-style`、`font-size`、`font-weight` 等；

<mark style="background: #BBFABBA6;">文本相关</mark>：`text-align`、`text-indent`、`text-decoration`、`text-shadow`、`letter-spacing`、`word-spacing`、`white-space`、`line-height`、`color` 等；

<mark style="background: #BBFABBA6;">列表相关</mark>：`list-style`、`list-style-image`、`list-style-type`、`list-style-position` 等；
[CSS列表（list-style） - CSS教程 - C语言网](https://www.dotcpp.com/course/1139)

<mark style="background: #BBFABBA6;">其他属性</mark>：`visibility`、`cursor` 等；

对于其他默认不继承的属性也可以通过以下几个属性值来控制继承行为：
- `inherit`：继承父元素对应属性的计算值；
- `initial`：应用该属性的默认值，比如 color 的默认值是 `#000`；
- `unset`：如果属性是默认可以继承的，则取 `inherit` 的效果，否则同 `initial`；
- `revert`：效果等同于 `unset`，兼容性差。

## 文档流

在CSS中，会把内容按照从左到右，从上到下的顺序进行排列显示

- 块级元素默认会占满整行，所以多个块级盒子之间是从上到下排列的；
- 内联元素默认会在一行里一列一列的排布，当一行放不下的时候，会自动切换到下一行继续按照列排布

**如何脱离文档流**
脱流文档流指节点脱流正常文档流后，在正常文档流中的其他节点将忽略该节点并填补其原先空间。文档一旦脱流，计算其父节点高度时不会将其高度纳入，脱流节点不占据空间。有两种方式可以让元素脱离文档流：<mark style="background: #FFF3A3A6;">浮动和定位</mark>。

- 使用浮动（float）会将元素脱离文档流，移动到容器左/右侧边界或者是另一个浮动元素旁边，该浮动元素之前占用的空间将被别的元素填补，另外浮动之后所占用的区域不会和别的元素之间发生重叠；
- 使用绝对定位或固定定位，也会使得元素脱离文档流

## 盒模型

在 CSS 中任何元素都可以看成是一个盒子，而一个盒子是由 4 部分组成的：内容（`content`）、内边距（`padding`）、边框（`border`）和外边距（`margin`）。

```css
.box {
    width: 200px;
    height: 200px;
    padding: 10px;
    border: 1px solid #eee;
    margin: 10px;
}
```
### W3C盒模型(标准盒模型)
盒子实际占据的宽 = 内容的宽width（content）+ border + padding + margin

![[Pasted image 20241021161554.png]]

box 元素内容宽度为 width = 200px
box 实际宽度 width + padding + border = 200 + 20 + 2 = 222
### IE盒模型（怪异盒模型
盒子实际占据的宽 = 内容的宽width（content+border+padding）+ margin

![[Pasted image 20241021162217.png]]

box元素所占宽度 width = 200px
内容宽度 = width - padding - border = 200 - 20 -2 = 178

### 盒子类型

盒子类型是由display决定，给一个元素设置 display 后，将会决定这个盒子的 2 个显示类型（display type）：
- outer display type（对外显示）：决定了该元素本身是如何布局的，即参与何种格式化上下文；
- inner display type（对内显示）：其实就相当于把该元素当成了容器，规定了其内部子元素是如何布局的，参与何种格式化上下文；、

#### outer display type

对外显示方面，盒子类型可以分成 2 类：block-level box（块级盒子） 和 inline-level box（行内级盒子）。
- 块级盒子：display 为 block、list-item、table、flex、grid、flow-root 等；
- 行内级盒子：display 为 inline、inline-block、inline-table 等；
所有块级盒子都会参与 BFC，呈现垂直排列；而所有行内级盒子都参会 IFC，呈现水平排列。

#### inner display type
对内方面，其实就是把元素当成了容器，里面包裹着文本或者其他子元素。container box 的类型依据 display 的值不同，分为 4 种：
- block container：建立 BFC 或者 IFC；
- flex container：建立 FFC；
- grid container：建立 GFC;
- ruby container：接触不多，不做介绍。

值得一提的是如果把 img 这种替换元素（replaced element）申明为 block 是不会产生 container box 的，因为替换元素比如 img 设计的初衷就仅仅是通过 src 把内容替换成图片，完全没考虑过会把它当成容器。

## 格式化上下文

格式化上下文(Formatting Context) ,是页面中的一块渲染区域，规定了渲染区域内部的子元素是如何排版以及相互作用的
- BFC (Block Formatting Context) 块级格式化上下文；
- IFC (Inline Formatting Context) 行内格式化上下文；
- FFC (Flex Formatting Context) 弹性格式化上下文；
- GFC (Grid Formatting Context) 格栅格式化上下文；

BFC - 详见
IFC - 详见

## 值和单位

### 值的类型
CSS的声明是由属性和值组成的，值的类型有很多种

- 数值： 长度值 ，用于指定例如元素 width、border-width、font-size 等属性的值
- 百分比：可以用于指定尺寸或长度，例如取决于父容器的 width、height 或默认的 font-size；
- 颜色：用于指定background-color、color
- 坐标位置：以屏幕的左上角为坐标原点定位元素的位置，比如常见的 background-position、top、right、bottom 和 left 等属性；
- 函数：用于指定资源路径或背景图片的渐变，比如 url()、linear-gradient() 等；
### 单位
#### px

**屏幕分辨率** 是指在屏幕纵横方向上的像素点数量，比如1920 * 1080，就是水平方向1920个像素，垂直方向1080个像素

px 表示的是 CSS 中的像素，在 CSS 中它是绝对的长度单位，也是最基础的单位，其他长度单位会自动被浏览器换算成 px。但是对于设备而言，它其实又是相对的长度单位，比如宽高都为 2px，在正常的屏幕下，其实就是 4 个像素点，而在设备像素比(devicePixelRatio) 为 2 的 Retina 屏幕下，它就有 16 个像素点。所以屏幕尺寸一致的情况下，屏幕分辨率越高，显示效果就越细腻。

**设备像素（Device pixels）**
设备屏幕的物理像素，表示的是屏幕的横纵有多少像素点；和屏幕分辨率是差不多的意思

**设备像素比（DPR）**
设备像素比表示 1 个 CSS 像素等于几个物理像素
计算公式：DPR = 物理像素数量  / 逻辑像素数量
浏览器中可以通过window.devicePixelRatio 来获取当前屏幕的DPR

**像素密度（DPI / PPI）**
像素密度也叫显示密度或者屏幕密度，缩写为 DPI(Dots Per Inch) 或者 PPI(Pixel Per Inch)。从技术角度说，PPI 只存在于计算机显示领域，而 DPI 只出现于打印或印刷领域。

计算公式：像素密度 = 屏幕对角线的像素尺寸 / 物理尺寸
比如对于，分辨率为750 * 1334 的iPhone6来说
```js
Math.sqrt(750 * 750 + 1334 * 1334) / 4.7 = 326ppi
```

**设备独立像素（DIP）**
DIP 是特别针对 Android设备而衍生出来的，原因是安卓屏幕的尺寸繁多，因此为了显示能尽量和设备无关，而提出的这个概念。它是基于屏幕密度而计算的，认为当屏幕密度是 160 的时候，px = DIP。

计算公式：dip = px * 160 / dpi

#### em

em 是 CSS 中相对长度单位中的一个

在font-size中使用是相对父元素的font-size大小，比如父元素 font-size: 16px，当给子元素指定 font-size: 2em 的时候，经过计算后它的字体大小会是 32px；

在其他属性中使用是相对于自身的字体大小，如 width/height/padding/margin 等

em 在计算的时候是会层层计算的
```html
<div>
    <p></p>
</div>

div { font-size: 2em; }
p { font-size: 2em; }

根元素 html 的字体大小是 16px，所以 p 标签最终计算出来后的字体大小会是 16 * 2 * 2 = 64px
```

#### rem

相对长度单位，相对于根元素html
UI设计图750px, Iphone X尺寸375px * 812px
```js
let padWidth = 750 // 设计图宽度
let clientWidth = document.documentElement.clientWidth
document.documentElement.style.fontSize = 200 * (clientWidth / padWidth) + 'px'
```
当视口是 375px 的时候，经过计算 html 的 font-size 会是 100px
每个从设计图量出来的尺寸只要除于 100 即可得到当前元素的 rem 值
把200换成2，设计图的10px就是10rem

#### vw vh
- 1vw = 视口宽度均分成 100 份中 1 份的长度；
- 1vh = 视口高度均分成 100 份中 1 份的长度；
JS 中 100vw = window.innerWidth，100vh = window.innerHeight。

vmin 取vw vh 中较小的值
vmax 取vw vh 中较大的值

## 颜色体系

### 颜色值类型

- 颜色关键字
- transparent关键字
- currentColor关键字
- RGB颜色
- HSL颜色

### 颜色关键字
列表：[CSS 色彩关键字(color keywords)](https://codepen.io/bulandent/pen/gOLovwL)

### transparent关键字
transparent关键字表示一个完全透明的颜色，从技术上说，它是带有 alpha 通道为最小值的黑色，是 rgba(0,0,0,0) 的简写。

写一个三角形
```css
div {
    border-top-color: #ffc107;
    border-right-color: #00bcd4;
    border-bottom-color: #e26b6b;
    border-left-color: #cc7cda;
    border-width: 50px;
    border-style: solid;
}
```

<mark style="background: #FFF3A3A6;">用 transparent 实现三角形的原理：</mark>

- 首先宽高必须是0px，通过边框的粗细来填充内容
- 哪条边需要就加上颜色，不需要的边则用transparent
- 想要什么样姿势的三角形，完全由上下左右 4 条边的中有颜色的边和透明的边的位置决定
- 等腰三角形：设置一条边有颜色，然后紧挨着的 2 边是透明，且宽度是有颜色边的一半；
- 直角三角形：设置一条边有颜色，然后紧挨着的任何一边透明即可。
示例：
![[Pasted image 20241022192037.png|595]]

<mark style="background: #FFF3A3A6;">增大点击区域</mark>

移动端的点击按钮区域比较小，由于现实效果又不太好把它做大，所以通过透明边框来增大按钮的点击区域
```css
.btn {
    border: 5px solid transparent;
}
```

### currentColor 关键字

currentColor 会取当前元素继承父级元素的文本颜色值或声明的文本颜色值
```css
.btn {
    color: red;
    border: 1px solid currentColor;
}
```

### RGBA

**十六进制**

RGB 中的每种颜色的值范围是 00~ff，值越大表示颜色越深。所以一个颜色正常是 6 个十六进制字符加上 # 组成，比如红色就是 `#ff0000`。

**函数符**
当 RGB 用函数表示的时候，每个值的范围是 0~255 或者 0%~100%，所以红色是 rgb(255, 0, 0)， 或者 rgb(100%, 0, 0)。

<mark style="background: #BBFABBA6;">使用函数来表示带不透明度的颜色值</mark>
0~1 及其之间的小数
0%~100%
67% 不透明度的红色是 rgba(255, 0, 0, 0.67) 或者 rgba(100%, 0%, 0%, 67%)

**注意**
RGB 的三个颜色值需要保持一致的写法，即数字和百分比不能混用
不透明度可以不用保持一致

**第四代CSS**
新增写法 rgba(255 0 0 / 0.67)

### HSL[A]
HSL[A] 颜色是由色相(hue)-饱和度(saturation)-亮度(lightness)-不透明度组成的颜色体系。
![[Pasted image 20241102204828.png|325]]

色相(H)：0-360 0deg - 360deg；0或360为红色，120 为绿色，240为蓝色
饱和度(S)：色彩的纯度，越高色彩越纯，低则逐渐变灰，取 0~100% 的数值；0% 为灰色， 100% 全色
亮度(L)：取 0~100%，0% 为暗，100% 为白
不透明度(A): 0-1 0-100%

在 Chrome DevTools 中可以按住 shift + 鼠标左键可以切换颜色的表示方式
## 媒体查询

媒体查询是指针对不同的设备、特定的设备特征或者参数进行定制化的修改网站样式
通过给<link> 加上media属性指定该样式文件对什么设备生效，不指定默认为all
```html
<link rel="stylesheet" src="styles.css" media="screen" />
<link rel="stylesheet" src="styles.css" media="print" />
```
支持的设备类型
- all：适用于所有设备；
- print：适用于在打印预览模式下在屏幕上查看的分页材料和文档；
- screen：主要用于屏幕；
- speech：主要用于语音合成器。
![[Pasted image 20241024162540.png]]

通过@media 让CSS规则在特定的条件生效
@media (min-width: 1000px) {}

媒体查询支持逻辑操作符：
- and：查询条件都满足的时候才生效；
- not：查询条件取反；
- only：整个查询匹配的时候才生效，常用语兼容旧浏览器，使用时候必须指定媒体类型；
- 逗号或者 or：查询条件满足一项即可匹配；