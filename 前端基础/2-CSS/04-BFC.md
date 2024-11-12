[CSS中的BFC详细讲解（易懂）\_css的bfc\_紫陌\~的博客-CSDN博客](https://blog.csdn.net/weixin_57677300/article/details/129046793)
### BFC全称叫做块级格式化上下文

一个隔离的独立容器，有自己的渲染规则，容器里面的元素不会在布局上影响到外面的元素

BFC是属于**普通流**的，我们可以把BFC看成页面的一块渲染区，他有自己的渲染规则，简单来说就是BFC可以看做元素的属性，当元素有了BFC这个属性，这个元素可以看做隔离了的容器，容器里面的元素不会在布局上影响到外面的元素。

### BFC 特性
1. 每一个BFC区域只包含其子元素，不包含其子元素的子元素.
2. 在BFC中，块级元素从顶端开始垂直地一个接一个的排列。（即便不在BFC里块级元素也会垂直排列）
3. bfc的区域不会与float的元素区域重叠。
4. 计算bfc的高度时，浮动元素也参与计算
5. bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。
6. 如果两个块级元素属于同一个BFC，它们的上下margin会重叠（或者说折叠），以较大的为准。但是如果两个块级元素分别在不同的BFC中，它们的上下边距就不会重叠了，而是两者之和。

### 创建BFC
1. 根元素(html)
2. 设置float浮动，不包含none
3. 绝对定位元素 (元素的 position 为 absolute 或 fixed)
4. display 为 inline-block、table-cell、table-caption、table、table-row、table-row-groutable-header-group、table-footer-group、inline-table、flow-root、flex 或 inline-flex、grid 或 inline-grid
5. 设置overflow，不为visible
6. contain 值为 layout、content 或 paint 的元素
7. 多列容器（column-count 或 column-width (en-US) 值不为 auto，包括column-count 为 1）


## BFC 作用

### 1. 避免外边距重叠
```js
.box{
    width: 200px;
    height: 200px;
    background: #5aa878;
    margin: 100px;
   }
<body>
    <div class="box"></div>
    <div class="box"></div>
</body>
```

结果是上间距100px的，这不是bug,这是一种规范。块的上外边距margin-top和下外边距margin-bottom会合并为单个边距，如果两个边距相等取其中一个，若大小边距不一样区最大边距，。
**解决**
```css
  .box{
    width: 150px;
    height: 150px;
    background-color: red;
    margin: 40px;
  }
  .container {
    overflow: hidden;
  }
 
 
<div class='container'>
    <div class="box"></div>
</div>
<div class="container">
    <div class="box"></div>
</div>
```

### 2. 清除浮动（父元素高度塌陷）
```js
.container{
    border: 2px solid yellowgreen;
}
.content{
    width: 100px;
    height: 100px;
    background: #47cabf;
    margin: 100px;
    float: left;
}
 
<div class="container">
    <div class="content"></div>
</div>
```

```js
.container{
    border: 2px solid yellowgreen;
    overflow: hidden;
}
.content{
    width: 100px;
    height: 100px;
    background: #47cabf;
    margin: 100px;
    float: left;
}
 
<div class="container">
    <div class="content"></div>
</div>
```

### 3. 防止元素被浮动元素覆盖

**可以制作两栏布局**
```js
.box1{
    width: 100px;
    height: 100px;
    background: blue;
    float: left;
}
.box2{
    width: 200px;
    height: 200px;
    background: red;
}
 
<div class="box1"></div>
<div class="box2"></div>
```
这里可以看到浮动元素覆盖了没有添加浮动的元素，如果想不被覆盖，可以触发正常的元素的BFC即可。所有在第二个元素添加overflow: hidden;属性，这样这两个属性就互不干扰。
```js
.box1{
    width: 100px;
    height: 100px;
    background: blue;
    float: left;
}
.box2{
    width: 200px;
    height: 200px;
    background: red;
    overflow: hidden;
}
 
<div class="box1"></div>
<div class="box2"></div>
```

### 4. 防止父子元素外边距塌陷
```js
.box1{
    width: 200px;
    height: 200px;
    background: blue;
}
.box2{
    width: 100px;
    height: 100px;
    background: red;
    margin-top: 20px;
}
 
<div class="box1">
   <div class="box2"></div>
</div>
```
![[Pasted image 20230818125041.png]]
给子元素添加margin-top:20px后，影响了父元素，给父元素添加BFC属性即可。
```js
.box1{
    width: 200px;
    height: 200px;
    background: blue;
    overflow: hidden;
}
.box2{
    width: 100px;
    height: 100px;
    background: red;
    margin-top: 20px;
}
 
<div class="box1">
   <div class="box2"></div>
</div>
```