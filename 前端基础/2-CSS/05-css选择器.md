[CSS的选择器(超详细)\_css选择器\_兰de宝贝的博客-CSDN博客](https://blog.csdn.net/weixin_48649246/article/details/127939139)
[CSS3选择器（详细！全！）\_白白卡路里的博客-CSDN博客](https://blog.csdn.net/weixin_54007670/article/details/127908932)
### 一、常用的四类选择器
1. 元素选择器
```js
p {
	color: red;
}
```
2. id选择器
3. 类选择器
4. 通配符选择器

### 二、复合选择器（两种）
1. 交集选择器
```js
/* 找到即是span，同时类名又是number的标签 */
 span.number{ 
	color:blue;
}
```
2. 并集选择器
```js
 span,.number{ 
	color:blue;
}
```

### 三、属性选择器

属性选择器可以根据元素的属性及属性值来选择元素
#### E[att^=value]

E[att^=value]属性选择器是指选择名称为E的标记，且该标记定义了att属性，att属性值包含**前缀**为value的子字符串。  
需要注意的是E是可以省略的，如果省略则表示可以匹配满足条件的任意元素。
```js
p[id^="one"]{ /*表示匹配包含id属性，且id属性值是以“one”字符串开头的p元素*/ 
	属性:属性值; 
}
```
#### E[att$=value]属性选择器
	指选择器名称为E的标记，且该标记定义了att属性，att属性值包含**后缀**为value的子字符串
	与E[att^=value]选择器一样，E元素可以省略的，如果省略则表示可以匹配满足条件的任意元素
#### E[att*=value]属性选择器
指选择器名称为E的标记，且该标记定义了att属性，att属性值**包含**value的子字符串。  
与前两个选择器一样，E元素可以省略的，如果省略则表示可以匹配满足条件的任意元素。
![[Pasted image 20230922180128.png]]
### 四、关系选择器
#### 后代选择器
```js
<style>
  /* 后代选择器(包含选择器),选择到的是box下面的所有后代p */
     .box p{
      width: 200px;
      height: 200px;
      background-color: yellow;
    } 
</style>

```
#### 子代选择器
```css
<style>
   /*子选择器选中的是.box下所有的儿子*/
   .box>p{
      width: 200px;
      height: 200px;
      background-color: yellow;
    } 
</style>
```
#### 相邻兄弟+
```js
<style>
   /* 相邻兄弟,会选择到box后面紧挨着的p,那么就是内容为111的p标签 */
    .box+p{
      width: 200px;
      height: 200px;
      background-color: yellow;
    }
</style>
```
#### 通用兄弟~

```js
<style>
 
   /*通用兄弟选择器,会选择到.box后面所有的兄弟p,那么就是除了内容为'44444'以外的所有p*/
   .box~p{
      width: 200px;
      height: 200px;
      background-color: yellow;
    }
 
</style>
```


### 五、伪类选择器
#### 01-常用的伪类选择器
:first-child   第一个子元素
:last-child    最后一个子元素
:nth-child()   选中第n个元素

关于:nth-child()的特殊值(括号内的内容可以填写以下几种)
>n   第n个   n的范围0到正无穷（全选）
>even或2n    选中偶数位的元素
>odd或2n+1   选中奇数位得到元素

:first-of-type  第一个子元素
:last-of-type   最后一个子元素
:nth-of-type()    选中第n个元素


```css
/* box里面的只有1个子元素是li的时候变 */
.box li:only-child{
  border: 2px solid blue;
}
/* box里面的li只有1个的时候变 */
.box li:only-of-type{
  border: 2px solid blue;
}
```

#### 02-否定伪类
:not() 将符号条件的元素去除
```css
<style type="text/css">
	body :not(h3){
		属性:属性值;
	}
</style>

<body>
	<h3>一<h3>
	<p>二</p>	/*选择*/
	<p>三</p>	/*选择*/
	<p>四</p>	/*选择*/
</body>


.box :not(#p1) {
	border: 1px solid red;
}
```

#### 03-:empty选择器
:empty选择器用来选择没有子元素或文本内容为空的所有元素。
```css
<style type="text/css">
	:empty{
		属性:属性值;
	}
</style>

<body>
	<p>一</p>
	<p>二</p>
	<p></p>	/*选择设置样式*/
	<p>三</p>
</body>

```

#### 04-:target
```css
<style type="text/css">
	:target{	/*用于为target元素设置样式，当单击链接时，所链接到的内容将会被添加样式*/
		属性:属性值;
	}
</style>

<body>
	<p><a href="#demo1">跳转至demo1</p>
	<p><a href="#demo2">跳转至demo2</p>
	<p>请点击上面的链接，:target选择器会突然显示当前活动的HTML锚</p>
	<p id="demo1">内容1</p>
	<p id="demo2">内容2</p>
</body>

```
:link        表示未访问过的a标签
:visited    表示访问过的a标签
以上两个伪类是超链接所独有的
由于隐私的问题，所以visited这个伪类只能修改链接的颜色
以下两个伪类是所有标签都可以使用
:hover        鼠标移入后元素的状态
:active        鼠标点击后，元素的状态 

### 六、伪元素选择器
常见的几个伪元素：
::first-letter    表示第一个字母
::first-line       表示第一行
::selection      表示选中的元素
::before          元素开始的位置前
::after             元素结束的位置后
#### 01-before选择器
```js
<style type="text/css">
	p::before
	{
		content:"文字"或"url()";
		属性:属性值;
	}
</style>
<body>
	<p>内容</p>
</body>

```
#### 02-after选择器
```css
<style type="text/css">
	p::after
	{
		content:"文字"或"url()";
		属性:属性值;
	}
</style>

<body>
	<p>内容</p>
</body>

```

### 七、伪类选择器和伪元素选择器的区别：
① 虚实不同
伪类选择器是实的，伪类选择器并不会创建元素，它只是修改已有元素的样式。
伪元素选择器是虚的，当它添加子元素时，新建了一个元素，并为其添加样式，但这个元素并不存在dom树中。
② 表示不同：
使用单冒号(:)表示伪类元素。
CSS3规范中要求使用双冒号(::)表示伪元素。


### css3选择器
![[Pasted image 20230818174730.png]]