[Flex 布局教程：语法篇 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?utm_source=tuicool)

## 概念

Flex 是 Flexible Box 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
任何一个容器都可以指定为 Flex 布局。
```css
.box{
  display: flex;
}
```

行内元素也可以使用 Flex 布局。
```css
.box{
  display: inline-flex;
}
```

Webkit 内核的浏览器，必须加上`-webkit`前缀
```css
.box{
  display: -webkit-flex; /* Safari */
  display: flex;
}
```
注意，设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效

## 容器的属性

以下6个属性设置在容器上。
```css
flex-direction
flex-wrap
flex-flow
justify-content
align-items
align-content
```

### flex-direction属性

```css
.box {
  flex-direction: row | row-reverse | column | column-reverse;
}
```

### flex-wrap属性

默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。
```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
`wrap-reverse`：换行，第一行在下方
![[Pasted image 20230819223334.png]]
### flex-flow属性

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

### justify-content属性

定义了项目在主轴上的对齐方式。
```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
- `flex-start`（默认值）：左对齐
- `flex-end`：右对齐
- `center`： 居中
- `space-between`：两端对齐，项目之间的间隔都相等。
- `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
![[Pasted image 20230819223512.png|500]]

### align-items属性

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

- `flex-start`：交叉轴的起点对齐。
- `flex-end`：交叉轴的终点对齐。
- `center`：交叉轴的中点对齐。
- `baseline`: 项目的第一行文字的基线对齐。
- `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
### align-content属性

[容易忽视的flex属性align-content - 知乎](https://zhuanlan.zhihu.com/p/448805816)
`align-content`属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用
```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
![[Pasted image 20230819223813.png]]

## 项目的属性

```css
order
flex-grow
flex-shrink
flex-basis
flex
align-self
```

### order属性
```css
.item {
  order: <integer>;
}
```

### flex-grow属性

**flex-grow** 属性定义项目的放大比例，**默认为0**，即如果存在剩余空间，也不放大。
```css
.item {
  flex-grow: <number>; /* default 0 */
}
```
如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

### flex-shrink属性

**flex-shrink**属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```
如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。
**负值对该属性无效**

### flex-basis属性
[CSS flex属性深入理解 « 张鑫旭-鑫空间-鑫生活](https://www.zhangxinxu.com/wordpress/2019/12/css-flex-deep/)
### flex属性

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。

建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。
### align-self属性
`align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```