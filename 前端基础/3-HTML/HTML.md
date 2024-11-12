[2022高频前端面试题——HTML（上篇） - 掘金](https://juejin.cn/post/7095899257072254989)
#### Doctype
`Doctype`是HTML5的文档声明，通过它可以告诉浏览器，使用哪一个HTML版本标准解析文档。在浏览器发展的过程中，HTML出现过很多版本，不同的版本之间格式书写上略有差异。如果没有事先告诉浏览器，那么浏览器就不知道文档解析标准是什么？此时，大部分浏览器将开启最大兼容模式来解析网页，我们一般称为`怪异模式`，这不仅会降低解析效率，而且会在解析过程中产生一些难以预知的`bug`，所以文档声明是必须的。

#### src 和 href 的区别
- **src：** 表示对资源的引用，它指向的内容会嵌入到当前标签所在的位置。src会将其指向的资源下载并应⽤到⽂档内，如请求js脚本。当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执⾏完毕，所以⼀般js脚本会放在页面底部。通常用于img、video、audio、script元素
- **href：** 表示超文本引用，它指向一些网络资源，建立和当前元素或本文档的链接关系。当浏览器识别到它他指向的⽂件时，就会并⾏下载资源，不会停⽌对当前⽂档的处理。 常用在a、link等标签上。

Link
[HTML link标签属性介绍和使用方法 - 文蚂蚁](https://wenmayi.com/post/323.html)
```html
<head>
	<link rel="stylesheet" type="text/css" href="theme.css" />
</head>
```

该标签包含属性type rel href 等
**type**  规定被链接文档的 MIME 类型。该属性最常见的 MIME 类型是 "text/css"
**rel** 规定当前文档与被链接文档之间的关系 取值stylesheet是指引用的文件是样式表
**href**  用来指定对应的url地址
**media** 规定被链接文档将显示在什么设备上，不同的媒介类型规定不同的样式 medida="screen"
#### 对HTML语义化的理解
HTML标签的语义化，简单来说，就是用正确的标签做正确的事情，给某块内容用上一个最恰当最合适的标签，使页面有良好的结构，页面元素有含义，无论是谁都能看懂这块内容是什么。

语义化的优点如下：
- 在没有CSS样式情况下也能够让页面呈现出清晰的结构
- 有利于SEO和搜索引擎建立良好的沟通，有助于爬虫抓取更多的有效信息，爬虫是依赖于标签来确定上下文和各个关键字的权重
- 方便团队开发和维护，语义化更具可读性，遵循W3C标准的团队都遵循这个标准，可以减少差异化

```html
<header></header>  头部

<nav></nav>  导航栏

<section></section>  区块（有语义化的div）

<main></main>  主要区域

<article></article>  主要内容

<aside></aside>  侧边栏

<footer></footer>  底部

```

#### DOCTYPE(⽂档类型) 的作⽤
DOCTYPE是HTML5中一种标准通用标记语言的文档类型声明，它的目的是**告诉浏览器（解析器）应该以什么样（html或xhtml）的文档类型定义来解析文档**，不同的渲染模式会影响浏览器对 CSS 代码甚⾄ JavaScript 脚本的解析。它必须声明在HTML⽂档的第⼀⾏。

#### img上 title 与 alt
- alt：全称`alternate`，切换的意思，如果无法显示图像，浏览器将显示alt指定的内容
- title：当鼠标移动到元素上时显示title的内容

#### 常⽤的meta标签有哪些
`meta` 标签由 `name` 和 `content` 属性定义，**用来描述网页文档的属性**，比如网页的作者，网页描述，关键词等，除了HTTP标准固定了一些`name`作为大家使用的共识，开发者还可以自定义name。
（1）`charset`，用来描述HTML文档的编码类型
```
<meta charset="UTF-8" >
```
（2） `keywords`，页面关键词：
```
<meta name="keywords" content="关键词" />
```
（3）`description`，页面描述：
```
<meta name="description" content="页面描述内容" />
```
（4）`refresh`，页面重定向和刷新：
```
<meta http-equiv="refresh" content="0;url=" />
```
content  内的数字表示时间(秒)，即多少秒后刷新，如果加url,则会重定向到指定网页（搜索引擎能够自动检测，也很容易被引擎视作误导而受到惩罚）。
（5）`viewport`，适配移动端，可以控制视口的大小和比例：
```
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
```
**width**：控制 viewport 的大小，可以指定的一个值或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
**height**：和width相对应，指定高度。
**initial-scale**：初始缩放。即页面初始缩放程度。这是一个浮点值，是页面大小的一个乘数。例如，如果你设置初始缩放为“1.0”，那么，web页面在展现的时候就会以target density分辨率的1:1来展现。如果你设置为“2.0”，那么这个页面就会放大为2倍。
**minimum-scale**：允许用户缩放到的最小比例
**maximum-scale**：最大缩放。即允许的最大缩放程度。这也是一个浮点值，用以指出页面大小与屏幕大小相比的最大乘数。例如，如果你将这个值设置为“2.0”，那么这个页面与target size相比，最多能放大2倍。
**user-scalable**：用户调整缩放。即用户是否能改变页面缩放程度。如果设置为yes则是允许用户对其进行改变，反之为no。默认值是yes。如果你将其设置为no，那么minimum-scale和maximum-scale都将被忽略，因为根本不可能缩放。

（6）搜索引擎索引方式：
```
<meta name="robots" content="index,follow" />
```
- `all`：文件将被检索，且页面上的链接可以被查询；
- `none`：文件将不被检索，且页面上的链接不可以被查询；
- `index`：文件将被检索；
- `follow`：页面上的链接可以被查询；
- `noindex`：文件将不被检索；
- `nofollow`：页面上的链接不可以被查询。
#### HTML5有哪些更新
##### 语义化标签
[你不知道的HTML5语义化标签 - 掘金](https://juejin.cn/post/6990572224637992996?searchId=20230913153003E6276688F519872ADECA)
```html
<article>: 定义文章的部分
<aside>: 标签用来表示跟当前页面的内容没有很相关的部分，通常用于显示侧边栏或者补充的内容。
<address>
<header>
<footer>
<nav>: 导航
<section>: 标签表示一个包含在HTML文档中的独立部分，它没有更具体的语义元素来表示。
<label>: 用于为 `<input>` 元素做标题绑定。
	<label for="name">姓名</label> 
	<input type="text" name="name" id="name" />

<progress> 标签表示进度条。它有一个 `max` 属性来设置进度条的最大值，以及一个 `value` 属性表示当前值。
```
**base**
```html
标签规定页面上所有链接的默认 URL 和默认目标，不同的是，它有点类似于 `<meta>` 标签，是写于 `<head>` 文档头部中的。

<base href="https://www.baidu.com" target="_blank">
<a href="">base</a>
```
**del ins**
```html
`<del>` 标签主要是 **delete** 的意思，即删除部分

`<ins>` 标签主要是 **insert** 的意思，即插入部分，是对删除部分的补充，同时，它还有两个属性
	<del>
	  <p>“v2.6.10”</p>
	</del>
	<ins cite="../howtobeawizard.html" datetime="2018-05">
	  <p>“v3.0.0”</p>
	</ins>
```
![[Pasted image 20230930170822.png]]
##### 媒体标签
（1） audio：音频
```html
<audio src='' controls autoplay loop='true'></audio>
- controls 控制面板
- autoplay 自动播放
- loop=‘true’ 循环播放
```

（2）video视频
```
<video src='' poster='imgs/aa.jpg' controls></video>
- poster：指定视频还没有完全下载完毕，或者用户还没有点击播放前显示的封面。默认显示当前视频文件的第一帧画面，当然通过poster也可以自己指定。
- controls 控制面板
- width
- height
```
（3）source标签 因为浏览器对视频格式支持程度不一样，为了能够兼容不同的浏览器，可以通过source来指定视频源。
```
<video>
 	<source src='aa.flv' type='video/flv'></source>
 	<source src='aa.mp4' type='video/mp4'></source>
 	您的浏览器不支持 vedio 元素。
</video>

```
##### 表单
[HTML新增表单元素和表单属性 - 知乎](https://zhuanlan.zhihu.com/p/360812821)
###### 表单类型
**email**：提交表单的时候验证输入值是否满足email的格式
```html
<input type="email" name="email"/>
```
**url**: 提交表单的时候验证输入值是否满足url的格式
**tel**: 验证输入的是否是电话号码的格式
**number**:根据你的设置提供选择数字的功能，其中min为最小值，max为最大值，value为默认值，step为点击箭头时数字的变化量，max、min、step、value均可不写，目前某些浏览器还不支持
```html
<input type="number" name="number" min=2 max=100 step=5 value="15"/>
```
**range**
会以一个滑块的形式表现包含一定范围内数字值的输入域，max为最大值，min为最小值，value为默认值，如果没有设置max和min，默认值是1-100

**color** ： 提供了一个颜色拾取器
```html
<input type="color" name="color"/>
```
**time** ： 时分秒
**date** ： 日期选择年月日
**datatime** ： 时间和日期(目前只有Safari支持)
**datatime-local** ：选取时间、日、月、年(本地时间)
**week** ：周控件
**month**：月控件
###### 表单属性
- placeholder ：提示信息
- autofocus ：自动获取焦点
- autocomplete=“on” 或者 autocomplete=“off” 使用这个属性需要有两个前提：
    - 表单必须提交过
    - 必须有name属性。
- required：要求输入框不能为空，必须有值才能够提交。
- pattern=" " 里面写入想要的正则模式，例如手机号patte="^(+86)?\d{10}$"
- multiple：可以选择多个文件或者多个邮箱
- form=" form表单的ID"
###### 表单事件：
- oninput 每当input里的输入框内容发生变化都会触发此事件。
- oninvalid 当验证不通过时触发此事件。

##### 进度条、度量器
- progress标签：用来表示任务的进度（IE、Safari不支持），max用来表示任务的进度，value表示已完成多少
- meter属性：用来显示剩余容量或剩余库存（IE、Safari不支持）
    - high/low：规定被视作高/低的范围
    - max/min：规定最大/小值
    - value：规定当前度量值
设置规则：min < low < high < max

##### DOM查询操作
- document.querySelector()
	element = document.querySelector('div#container');//返回id为container的首个div
	element = document.querySelector('.foo,.bar');//返回带有foo或者bar样式类的首个元素
- document.querySelectorAll()
	该方法返回所有满足条件的元素，结果是个nodeList集合。
	elements = document.querySelectorAll('div.foo');//返回所有带foo类样式的div
##### Web存储
HTML5 提供了两种在客户端存储数据的新方法：

- localStorage - 没有时间限制的数据存储
- sessionStorage - 针对一个 session 的数据存储

##### 拖放和画布

#### 行内元素有哪些？块级元素有哪些？ 空(void)元素有那些？

- 行内元素有：`a b span img input select strong`；
- 块级元素有：`div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p`；
空元素，即没有内容的HTML元素。空元素是在开始标签中关闭的，也就是空元素没有闭合标签：
- 常见的有：`<br>`、`<hr>`、`<img>`、`<input>`、`<link>`、`<meta>`；
- 鲜见的有：`<area>`、`<base>`、`<col>`、`<colgroup>`、`<command>`、`<embed>`、`<keygen>`、`<param>`、`<source>`、`<track>`、`<wbr>`。

#### Canvas和SVG的区别

**（1）SVG：** SVG可缩放矢量图形（Scalable Vector Graphics）是基于可扩展标记语言XML描述的2D图形的语言，SVG基于XML就意味着SVG DOM中的每个元素都是可用的，可以为某个元素附加Javascript事件处理器。在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

其特点如下：

- 不依赖分辨率
- 支持事件处理器
- 最适合带有大型渲染区域的应用程序（比如谷歌地图）
- 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
- 不适合游戏应用
**（2）Canvas：** Canvas是画布，通过Javascript来绘制2D图形，是逐像素进行渲染的。其位置发生改变，就会重新进行绘制。

其特点如下：

- 依赖分辨率
- 不支持事件处理器
- 弱的文本渲染能力
- 能够以 .png 或 .jpg 格式保存结果图像
- 最适合图像密集型的游戏，其中的许多对象会被频繁重绘

注：矢量图，也称为面向对象的图像或绘图图像，在数学上定义为一系列由线连接的点。矢量文件中的图形元素称为对象。每个对象都是一个自成一体的实体，它具有颜色、形状、轮廓、大小和屏幕位置等属性。



cookie
[01.浅谈浏览器存储(cookie,localStorage,sessionStorage) - 掘金](https://juejin.cn/post/6982574541943865357?searchId=202309131551035CC520CB6076042D4035)