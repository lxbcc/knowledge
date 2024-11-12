[「2021」高频前端面试题汇总之CSS篇 - 掘金](https://juejin.cn/post/6905539198107942919?searchId=202308142037592BBB164529AEF72C1A20)
[CSS入门教程 - C语言网](https://www.dotcpp.com/course/css/)

## 01-CSS基础
### CSS三大特性
1. **层叠性**
   相同选择器给设置相同的样式，此时一个样式就会覆盖（层叠）另一个冲突的样式。层叠性主要解决样式冲突的问题。
   层叠性原则：
   - 样式冲突，遵循的原则是就近原则，哪个样式离结构近，就执行哪个样式
   - 样式不冲突，不会层叠
2. **继承性**
   CSS中的继承：子标签会继承父标签的某些样式，如文本颜色和字号。简单的理解就是：子承父业。
3. **优先级**
   当同一个元素指定多个选择器，就会有优先级的产生。
   - 选择器相同，则执行层叠性
   - 选择器不同，则根据选择器权重执行

### CSS 选择器及其优先级

![[Pasted image 20230814204134.png|625]]
对于选择器的**优先级**：
- 内联样式：1000
- id 选择器：100
- 类选择器、伪类选择器、属性选择器：10
- 标签选择器、伪元素选择器：1

**注意事项：**
- !important声明的样式的优先级最高；
- 如果优先级相同，则最后出现的样式生效；
- 继承得到的样式的优先级最低；
- 通用选择器（ * ）、子选择器（>）和相邻同胞选择器（+）并不在这四个等级中，所以它们的权值都为 0 ；
- 样式表的来源不同时，优先级顺序为：内联样式 > 内部样式 > 外部样式 > 浏览器用户自定义样式 > 浏览器默认样式。

### CSS中可继承与不可继承属性有哪些

**一、无继承性的属性**
1. **display**：规定元素应该生成的框的类型
2. **文本属性**：
   - vertical-align：垂直文本对齐
   - text-decoration：规定添加到文本的装饰
   - text-shadow：文本阴影效果
   - white-space：空白符的处理
   - unicode-bidi：设置文本的方向

3. **盒子模型的属性**：width、height、margin、border、padding
4. **背景属性**：background、background-color、background-image、background-repeat、background-position、background-attachment
5. **定位属性**：float、clear、position、top、right、bottom、left、min-width、min-height、max-width、max-height、overflow、clip、z-index
6. **生成内容属性**：content、counter-reset、counter-increment
7. **轮廓样式属性**：outline-style、outline-width、outline-color、outline
8. **页面样式属性**：size、page-break-before、page-break-after
9. **声音样式属性**：pause-before、pause-after、pause、cue-before、cue-after、cue、play-during

**二、有继承性的属性**
1. **字体系列属性**
   - font-family：字体系
   - font-weight：字体的粗
   - font-size：字体的大小
   - font-style：字体的风格
2. **文本系列属性**
   - text-indent：文本缩进
   - text-align：文本水平对齐
   - line-height：行高
   - word-spacing：单词之间的间距
   - letter-spacing：中文或者字母之间的间距
   - text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）
   - color：文本颜色
3. **元素可见性**
   - visibility：控制元素显示隐藏
4. **列表布局属性**
   - list-style：列表风格，包括list-style-type、list-style-image等
   - [CSS列表（list-style） - CSS教程 - C语言网](https://www.dotcpp.com/course/1139)
5. **光标属性**
   - cursor：光标显示为何种形态

### 盒子模型

盒子模型的组成为：`content（元素内容）` + `padding（内边距）` + `border（边框）` + `margin（外边距）`
- **W3C盒模型**(标准盒模型)：盒子实际总宽高 = 内容的宽高width\height（content）+ border + padding + margin
- **IE盒模型**（怪异盒模型）：盒子实际总宽高 = 内容的宽高width\height（content+border+padding）+ margin
box-sizing: content-box  标准盒模型
box-sizing: border-box  怪异盒模型

### px em rem
**px** 全称`pixel`像素，是相对于屏幕分辨率而言的，它是一个绝对单位，但同时具有一定的相对性。因为在同一个设备上每个像素代表的物理长度是固定不变的，这点表现的是绝对性。但是在不同的设备之间每个设备像素所代表的物理长度是可以变化的，这点表现的是相对性

**em**是一个相对长度单位，具体的大小需要相对于父元素计算，比如父元素的字体大小为80px，那么子元素1em就表示大小和父元素一样为80px，0.5em就表示字体大小是父元素的一半为40px
如果当前元素没有定义font-size，那该元素会继承父元素的font-size，如果父元素的font-size为16px，那么当前元素的font-zise也就为16px。<mark class="hltr-orange">em是相对于自身的font-size</mark>，如果当前元素的font-size为16px，width为10em，那么也就是width为160px。

**rem**是CSS3新增的一个相对单位（root em，根em），它是相对于根（浏览器）字体的。注意：不同浏览器默认的font-size值不同。

### vw vh
**vw**是Viewport width的缩写，代表视口的宽度。<mark class="hltr-cyan">1vw等于视口宽度的1%</mark>。例如，将一个元素的宽度设置为50vw，就是将其宽度设置为视口宽度的50%。如果视口宽度为1000px，则该元素的宽度为500px。

**vh**是Viewport height的缩写，代表视口的高度。<mark class="hltr-cyan">1vh等于视口高度的1%</mark>。例如，将一个元素的高度设置为50vh，就是将其高度设置为视口高度的50%。如果视口高度为800px，则该元素的高度为400px。

### display的属性值及其作用
![[Pasted image 20230814205116.png]]
**none**: 从文档流中删除
**block**: 会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；
**inline**: 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
**inline-block**: 将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内,设置, 设置padding-top margin-top 整个一行都会位移 
**list-item**: 会把元素作为列表显示
**table**: 元素会作为块级表格来显示，子元素display: table-cell
**inherit**: 从父元素继承display属性值
### display的block、inline和inline-block的区别
（1）**block：** 会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；
（2）**inline：** 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
（3）**inline-block：** 将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内

对于行内元素和块级元素，其特点如下：
**（1）行内元素**
- 设置宽高无效；
- 可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
- 不会自动换行；
**（2）块级元素**
- 可以设置宽高；
- 设置margin和padding都有效；
- 可以自动换行；
- 多个块状，默认排列从上到下
![[Pasted image 20230814210302.png]]
###  隐藏元素的方法有哪些
- **display: none**：渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的监听事件。
- **visibility: hidden**：元素在页面中仍占据空间，但是不会响应绑定的监听事件。
- **opacity: 0**：将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。
- **position: absolute**：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。
- **z-index: 负值**：来使其他元素遮盖住该元素，以此来实现隐藏。
- **clip/clip-path** ：使用元素裁剪的方法来实现元素的隐藏，这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。
- **transform: scale(0,0)**：将元素缩放为 0，来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件
### 分析比较 opacity: 0、visibility: hidden、display: none 优劣和适用场景
1. **结构**： 
   display:none: 会让元素完全从渲染树中消失，渲染的时候不占据任何空间, 不能点击， 
   visibility: hidden:不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，不能点击 
   opacity: 0: 不会让元素从渲染树消失，渲染元素继续占据空间，只是内容不可见，可以点击
2. **继承**：
   display: none和opacity: 0：是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示。 
   visibility: hidden：是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式。
3. **性能**：
   displaynone : 修改元素会造成文档回流,读屏器不会读取display: none元素内容，性能消耗较大
   visibility:hidden: 修改元素只会造成本元素的重绘,性能消耗较少读屏器读取visibility: hidden元素内容 
   opacity: 0 ： 修改元素会造成重绘，性能消耗较少
### clip-path
[css中clip属性 - 浅笑· - 博客园](https://www.cnblogs.com/qianxiaox/p/13761346.html)

### link和@import的区别
1. 属性不同
   link是HTML提供的标签，不仅可以加载css文件，还能定义 RSS、rel 连接属性等。而@import是css中的语法规则
2. 加载顺序不同
   页面打开时，link引用的css文件被加载。而@import引用的CSS等页面加载完后最后加载。
3. 兼容性
   @import是css2.1后提出的，而link是不存在兼容问题。
4. DOM控制性
   js操作DOM，可以使用link改变样式，无法使用@import的方式使用样式。

### CSS3中有哪些新特性
[[07-CSS3新特性]]
- 选择器
- 盒子模型属性：border-radius、box-shadow、border-image
- 背景：background-size、background-origin、background-clip
- 文本效果：text-shadow、word-wrap
- 颜色：新增 RGBA，HSLA模式 
- 渐变：线性渐变、径向渐变
- 字体：@font-face
- 2D/3D转换：transform、transform-origin
- 过渡与动画：transition、@keyframes、animation
- 多列布局 
- 媒体查询
[CSS3新特性 - \_tianYou - 博客园](https://www.cnblogs.com/tiyou/p/16172145.html)

### 对line-height 的理解及其赋值方式
1. **line-height的概念：**
   - line-height 指一行文本的高度，包含了字间距，实际上是下一行基线到上一行基线距离；
   - 如果一个标签没有定义 height 属性，那么其最终表现的高度由 line-height 决定；
   - 一个容器没有设置高度，那么撑开容器高度的是 line-height，而不是容器内的文本内容；
   - 把 line-height 值设置为 height 一样大小的值可以实现单行文字的垂直居中；
   - line-height 和 height 都能撑开一个高度；
2. **line-height 的赋值方式：**
   - 带单位：px 是固定值，而 em 会参考父元素 font-size 值计算自身的行高
   - 纯数字：会把比例传递给后代。例如，父级行高为 1.5，子元素字体为 18px，则子元素行高为 1.5 * 18 = 27px
   - 百分比：将计算后的值传递给后代

### CSS 优化和提高性能的方法有哪些
**加载性能：**
（1）css压缩：将写好的css进行打包压缩，可以减小文件体积。
（2）css单一样式：当需要下边距和左边距的时候，很多时候会选择使用 margin:0 0；但margin-bottom:0;margin-left:0;执行效率会更高。
（3）减少使用@import，建议使用link，因为后者在页面加载时一起加载，前者是等待页面加载完成之后再进行加载
**选择器性能：**
**渲染性能：**
### 如何判断元素是否到达可视区域
以图片显示为例：

- `window.innerHeight` 是浏览器可视区的高度；
- `document.body.scrollTop || document.documentElement.scrollTop` 是浏览器滚动的过的距离；
- `imgs.offsetTop` 是元素顶部距离文档顶部的高度（包括滚动条的距离）；
- 内容达到显示区域的：**img.offsetTop < window.innerHeight + document.body.scrollTop**
  ![[Pasted image 20230814223407.png|600]]


### 重绘与重排(回流)的区别
**重排:** 部分渲染树（或者整个渲染树）需要重新分析并且节点尺寸需要重新计算，表现为重新生成布局，重新排列元素
**重绘:** 由于节点的几何属性发生改变或者由于样式发生改变，例如改变元素背景色时，屏幕上的部分内容需要更新，表现为某些元素的外观被改变

单单改变元素的外观，肯定不会引起网页重新生成布局，但当浏览器完成重排之后，将会重新绘制受到此次重排影响的部分

重排和重绘代价是高昂的，它们会破坏用户体验，并且让UI展示非常迟缓，而相比之下重排的性能影响更大，在两者无法避免的情况下，一般我们宁可选择代价更小的重绘。

『重绘』不一定会出现『重排』，『重排』必然会出现『重绘』。
**导致重排**
1. 改变窗口大小
2. 添加或者删除DOM元素
3. 修改DOM元素的位置、尺寸、边距等
4. 改变元素的字体大小
5. 显示或隐藏一个DOM元素
6. 改变元素的class属性
**导致重绘**
1. 修改DOM元素的背景、颜色、字体等属性
2. 改变元素的文本内容
3. 修改元素的边框、圆角等属性
**一些优化方案**
1. 避免逐项更改样式，使用class类对样式进行统一更改，减少回流。
2. 避免循环操作DOM。
3. 尽量避免多次获取offsetXXX、scrollXXX、clientXXX等属性，如果不能避免则用变量保存来尽量减少回流。
4. 动画效果应用于position为absolute或fixed的元素上，动画因为脱离文档流，减少回流与重绘。
5. 为动画开启GPU加速，开启css3动画加速的本质是渲染元素图层在GPU中 transform 属性不会触发重绘。所以尽量使用 transform: translate() 代替left与top进行动画。
### ::before 和 :after 的双冒号和单冒号有什么区别？

（1）冒号(`:`)用于`CSS3`伪类，双冒号(`::`)用于`CSS3`伪元素。 （2）`::before`就是以一个子元素的存在，定义在元素主体内容之前的一个伪元素。并不存在于`dom`之中，只存在在页面之中。

**注意：** `:before` 和 `:after` 这两个伪元素，是在`CSS2.1`里新出现的。起初，伪元素的前缀使用的是单冒号语法，但随着`Web`的进化，在`CSS3`的规范里，伪元素的语法被修改成使用双冒号，成为`::before`、`::after`。

### display:inline-block 什么时候会显示间隙？
- 有空格时会有间隙，可以删除空格解决
```js
<div class="child1">11111</div><div class="child2">22222</div>
```
- margin正值时，可以让margin使用负值解决；
- 使用font-size时，可通过设置`font-size:0`、`letter-spacing`、`word-spacing`解决；
![[Pasted image 20230817211703.png]]
![[Pasted image 20230817211717.png]]
### 单行、多行文本溢出隐藏

```css
	.visible {
		/* 超出部分溢出显示 */
		overflow: visible;
	}
	.hidden {
		/* 超出部分被隐藏 */
		overflow: hidden;
	}
	.scroll {
		/* 水平垂直都出现滚动条，可以滚动显示*/
		overflow: scroll;
	}
	.auto {
		/* 超出部门出现滚动条，未超出部门可以正常显示 */
		overflow: auto;
	}
```
**单行文本溢出省略**
单行文本溢出省略通常用于标题等文本显示，可以通过设置white-space和text-overflow属性实现。
```js
<style>
  .overflow {
	width: 100px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>
<div class="overflow">这是一段需要溢出省略的文本内容</div>
```
![[Pasted image 20231002151158.png]]
**多行文本溢出省略**
-webkit-line-clamp属性：用来限制显示的行数。

display属性：用来设置容器的display属性为-webkit-box，使其变成一个块级盒子。

-webkit-box-orient属性：用来设置块级盒子的排列方向为垂直方向。
```js
<style>
  .overflow {
	width: 100px;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>
<div class="overflow">这是一段需要溢出省略的多行文本内容，如果文本过长会出现省略号</div>
```

多行省略主要是用来实现在固定高度的容器中，将超出容器高度的文本省略掉
```js
<style>
  .ellipsis {
	width: 100px;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    line-height: 25px;
    height: 50px;
  }
</style>
<div class="ellipsis">这是一段需要省略的多行文本内容，如果文本过长会出现省略号</div>
 
```

```js
<style>
  .ellipsis::before {
    content: "...";
    position: absolute;
    bottom: 0;
    right: 0;
    padding-left: 10px;
    background: white;
  }
  .ellipsis {
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    line-height: 25px;
    height: 50px;
  }
</style>
<div class="ellipsis">这是一段需要省略的多行文本内容，如果文本过长会出现省略号</div>
 
```
### transform
```
translate(x,y)水平方向和垂直方向同时移动（也就是X轴和Y轴同时移动）
translateX(x)仅水平方向移动（X轴移动）
translateY(Y)仅垂直方向移动（Y轴移动）
transform:scale(0.8,1);
```

### 浏览器解析页面主要分为以下的流程：

1. 解析HTML，形成HTML DOM树

2. 解析CSS，生成CSS规则树

3. 将HTML DOM树与CSS规则树结合（attachment），生成Render树

4. 布局Render树（layout/reflow），负责各元素大小、位置的计算

5. 绘制Render树（painting），绘制页面像素信息

6. 浏览器将各层信息发送给GPU，GPU将各层合成，显示在屏幕上
## 02-布局
### 水平垂直居中布局

[[02-水平垂直居中]]
### 常见的CSS布局单位

常用的布局单位包括<mark class="hltr-yellow">像素px</mark>，<mark class="hltr-yellow">百分比%</mark>，<mark class="hltr-yellow">em，rem，vw/vh</mark>
1. **像素**（`px`）是页面布局的基础，一个像素表示终端（电脑、手机、平板等）屏幕所能显示的最小的区域，像素分为两种类型：CSS像素和物理像素：
   - **CSS像素**：为web开发者提供，在CSS中使用的一个抽象单位；
   - **物理像素**：只与设备的硬件密度有关，任何设备的物理像素都是固定的。

2. **百分比**（`%`），当浏览器的宽度或者高度发生变化时，通过百分比单位可以使得浏览器中的组件的宽和高随着浏览器的变化而变化，从而实现响应式的效果。一般认为子元素的百分比相对于直接父元素。

3. **em和rem** 相对于px更具灵活性，它们都是相对长度单位，它们之间的区别：**em相对于父元素，rem相对于根元素。**
   - **em：** 文本相对长度单位。相对于当前对象内文本的字体尺寸。如果当前行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸(默认16px)。(相对父元素的字体大小倍数)。
   - **rem：** rem是CSS3新增的一个相对单位，相对于根元素（html元素）的font-size的倍数。**作用**：利用rem可以实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值来设置font-size的值，以此实现当屏幕分辨率变化时让元素也随之变化。

4. **vw/vh**是与视图窗口有关的单位，vw表示相对于视图窗口的宽度，vh表示相对于视图窗口高度，除了vw和vh外，还有vmin和vmax两个相关的单位。
   - vw：相对于视窗的宽度，视窗宽度是100vw；
   - vh：相对于视窗的高度，视窗高度是100vh；
   - vmin：vw和vh中的较小值；
   - vmax：vw和vh中的较大值；

**vw/vh** 和百分比很类似，两者的区别：
- 百分比（`%`）：大部分相对于祖先元素，也有相对于自身的情况比如（border-radius、translate等)
- vw/vm：相对于视窗的尺寸

### rem原理

[rem的实现原理\_rem原理\_QY\_NO.1的博客-CSDN博客](https://blog.csdn.net/weixin_37739550/article/details/119824008)

em和rem在逻辑上的差别仅仅是参照对象不同，em是参照父元素的大小，rem是参照根元素html的大小
rem布局实际上就是想实现等比例缩放
rem设计原理
**我们设计稿的宽度是750px，那么对于设计稿上宽度为600px的一个元素，它的rem值是怎么计算呢？**
1. 上面我们把屏幕分成了10份，屏幕宽度就是10rem
2. 设计稿上600px元素在设计稿上占的比例是 600/750，将这个比例应用到屏幕上，屏幕的宽度是10rem，那么，这个元素在屏幕上占用的rem就是 600/750 * 10rem
3. 所以，公式就是 设计稿元素尺寸/设计稿宽度 * 以rem为单位的屏幕宽度

但是字体的大小和宽度是不可以用rem来确定的，字体大小和宽度不成线性关系。
解决办法：在body上做修正*
html {fons-size: width / 100}  
body {font-size: 16px}

### 两栏布局的实现

[CSS多栏布局-两栏布局和三栏布局\_css flex 两列-CSDN博客](https://blog.csdn.net/chaoPerson/article/details/130665235)
1. 浮动 + margin
2. 浮动 + BFC [[04-BFC]]
3. 定位
4. Flex
### 三栏布局的实现
![[Pasted image 20231002155459.png]]
1. 浮动 + margin
```js
<style>
    .container {
      width: 500px;
      height: 200px;
      line-height: 200px;
    }
    .left {
      width: 100px;
      height: 100%;
      float: left;
      background: orange;
      text-align: center;
    }
    .right {
      width: 100px;
      height: 100%;
      float: right;
      background: blue;
      text-align: center;
    }
 
    .center {
      height: 100%;
      margin-left: 100px;
      margin-right: 100px;
      background: oldlace;
    }
  </style>
  <div class="container">
    <div class="left">left固定</div>
    <div class="right">right固定</div>
    <div class="center">中间自适应</div>
  </div>
```

浮动 + BFC
定位
Flex
### Flex布局
[[03-Flex弹性布局]]

任何一个容器都可以指定为Flex布局。行内元素也可以使用Flex布局。注意，设为Flex布局以后，**子元素的float、clear和vertical-align属性将失效**

以下6个属性设置在**容器上**：

- flex-direction属性决定主轴的方向（即项目的排列方向）。
- flex-wrap属性定义，如果一条轴线排不下，如何换行。
- flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
- justify-content属性定义了项目在主轴上的对齐方式。
- align-items属性定义项目在交叉轴上如何对齐。
- align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

以下6个属性设置在**项目上**：

- order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。
- flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
- flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
- flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
- flex属性是flex-grow，flex-shrink和flex-basis的简写，默认值为0 1 auto。
- align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

## 03-定位与浮动

### 为什么需要清除浮动

**浮动的定义：** 非IE浏览器下，容器不设高度且子元素浮动时，容器高度不能被内容撑开。 此时，内容会溢出到容器外面而影响布局。这种现象被称为浮动（溢出）。

**浮动的工作原理：**
- 浮动元素脱离文档流，不占据空间（引起“高度塌陷”现象）
- 浮动元素碰到包含它的边框或者其他浮动元素的边框停留

![[Pasted image 20240417164344.png]]
### 清除浮动的方法

[实现清除浮动的几种方式 - 知乎](https://zhuanlan.zhihu.com/p/389386268)
1. clear 清除浮动（添加空div法）在浮动元素下方添加空div，并给该元素写css样式：{clear:both;}
```css
  .container{
    width: 300px;
    background-color: rgb(128, 185, 214);
  }
  .box1{
    width: 100px;
    height: 100px;
    background-color: rgb(65, 37, 37);
    float: left;
  }
  .box2{
    clear: both;
  }
```

2. 给浮动元素父级设置高度
3. 父级同时浮动（需要给父级同级元素添加浮动）
```css
  .container{
    width: 300px;
    background-color: rgb(128, 185, 214);
    float: left;
  }
  .box1{
    width: 100px;
    height: 100px;
    background-color: rgb(65, 37, 37);
    float: left;
  }
```
4. 父级设置成inline-block，其margin: 0 auto居中方式失效
5. 给父级添加overflow:hidden 清除浮动方法
```css
.container{
  width: 300px;
  background-color: rgb(128, 185, 214);
  overflow: hidden;
}
.box1{
  width: 100px;
  height: 100px;
  background-color: rgb(65, 37, 37);
  float: left;
}
```
6. 万能清除法 ::after 伪元素清浮动（现在主流方法，推荐使用）
   给父级元素添加伪元素
```css
.clearfix::after{	
	display: block;  /* 使其成为块级元素后*/
	content: "";    /*为伪元素加入空内容，以便伪元素中不会有内容显示在页面中*/
	height: 0;       /* 为使伪元素不影响页面布局，将伪元素高度设置为0*/
	clear: both;     /* 清除浮动*/
	}
.clearfix { *zoom:1; }  /*兼容IE6、IE7 */
```

### BFC
[[04-BFC]]
BFC全称叫做<mark class="hltr-cyan">块级格式化上下文</mark>
块格式化上下文（Block Formatting Context，BFC）是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。

**创建BFC的条件：**
- 根元素：body；
- 元素设置浮动：float 除 none 以外的值；
- 元素设置绝对定位：position (absolute、fixed)；
- display 值为：inline-block、table-cell、table-caption、flex等；
- overflow 值为：hidden、auto、scroll；
  
**BFC的特点：**
- 垂直方向上，自上而下排列，和文档流的排列方式一致。
- 在BFC中上下相邻的两个容器的margin会重叠
- 计算BFC的高度时，需要计算浮动元素的高度
- BFC区域不会与浮动的容器发生重叠
- BFC是独立的容器，容器内部元素不会影响外部元素
- 每个元素的左margin值和容器的左border相接触

**BFC的作用：**
- **解决margin的重叠问题**：由于BFC是一个独立的区域，内部的元素和外部的元素互不影响，将两个元素变为两个BFC，就解决了margin重叠的问题。
- **解决高度塌陷的问题**：在对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为0。解决这个问题，只需要把父元素变成一个BFC。常用的办法是给父元素设置`overflow:hidden`。
- **创建自适应两栏布局**：可以用来创建自适应两栏布局：左边的宽度固定，右边的宽度自适应。

### 介绍下 BFC、IFC、GFC 和 FFC

- BFC：块级格式上下文，指的是一个独立的布局环境，BFC 内部的元素布局与外部互不影响。
- IFC：行内格式化上下文，将一块区域以行内元素的形式来格式化。
- GFC：网格布局格式化上下文，将一块区域以 grid 网格的形式来格式化
- FFC：弹性格式化上下文，将一块区域以弹性盒的形式来格式化

### position的属性有哪些，区别是什么

![[Pasted image 20230815202159.png]]

### 对 sticky 定位的理解
sticky 英文字面意思是粘贴，所以可以把它称之为粘性定位。语法：**position: sticky;** 基于用户的滚动位置来定位。
粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。它的行为就像 **position:relative;** 而当页面滚动超出目标区域时，它的表现就像 **position:fixed;**，它会固定在目标位置。元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

## 04-其他
### 如何优化图片
1. 对于很多装饰类图片，尽量不用图片，因为这类修饰图片完全可以用 CSS 去代替。
2. 对于移动端来说，屏幕宽度就那么点，完全没有必要去加载原图浪费带宽。一般图片都用 CDN 加载，可以计算出适配屏幕的宽度，然后去请求相应裁剪好的图片。
3. 小图使用 _base64_ 格式
4. 将多个图标文件整合到一张图片中（雪碧图）
5. 选择正确的图片格式：

- 对于能够显示 WebP 格式的浏览器尽量使用 WebP 格式。因为 WebP 格式具有更好的图像数据压缩算法，能带来更小的图片体积，而且拥有肉眼识别无差异的图像质量，缺点就是兼容性并不好
- 小图使用 PNG，其实对于大部分图标这类图片，完全可以使用 SVG 代替
- 照片使用 JPEG

### 你能描述一下渐进增强和优雅降级之间的不同吗