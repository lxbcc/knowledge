[你很懂闭包嘛 - 掘金](https://juejin.cn/post/7248157339811823672?searchId=20230823172357BCCD2AF3BB78A73860FA)
## 执行上下文

### var的设计缺陷

```js
foo() // 函数foo被被执行
console.log(myname) // undefined
var myname = 'lxy'
function foo() {
   console.log('函数foo被执行');
}

for (var i = 0; i < 5; i++) {
  setTimeout(() => {
    console.log(i); // 输出5个5
  }, 0);
}
console.log(i); // 5
```

上面代码的输入预期与常识不符:
1. 函数和变量在声明之前就可以使用操作了
2. for循环代码块执行完成，里面声明的变量没有被销毁，在外部仍然可以被访问到

### 执行上下文
简单来说执行上下文是js执行一段代码时的运行环境。当js执行一段可执行代码时，会创建对应的执行上下文

执行上下文有三种类型：
1. 全局执行上下文
2. 函数执行上下文
3. eval 产生的执行上下文

### 预解析
[js预解析 - 小黄耗子 - 博客园](https://www.cnblogs.com/mijuntao/p/15073657.html)
<mark class="hltr-cyan">var 预解析</mark>
js 在正常解析之前，会快速的把 script (或者 funciton )中的 var 声明及声明的名字，提升到代码块(作用域)的最前面。
<mark class="hltr-cyan">function预解析</mark>
js 在正常解析之前，会快速的把 script (或者 funciton )中的 function 及内容提升到代码块(作用域)的最前面，跟在 var 声明之后。

```js
var a = 10;
a = function(){};
console.log(a); //function (){}
var定义的a，是同一个a，不会产生不同的变量，只是让a改变了值，由数值变成了函数。
```

```js
var a = function(){};
var a = 10;
console.log(a); //10

var声明按照赋值顺序执行，同名的var声明，后者的声明不会产生新变量，只是值会覆盖前者。
```

预解析，不会超出script标签，前面访问不到后面的定义
后面script标签中可以访问呢前面script标签中的js,因为js是从上至下执行的。
函数内部同名变量的声明高于传入的同名参数
函数内参数的声明高于外部同名变量的声明。
## 预编译
[【经典面试题】JS作用域、作用域链以及预编译 - 掘金](https://juejin.cn/post/7217100608775667773?from=search-suggest#heading-11)
[轻松get——js预编译，作用域以及作用域链 - 掘金](https://juejin.cn/post/7217057805782270009?searchId=20231009171946B3A5AFAFC2DCA631248E)

在上下文创建以后, 并不会立即执行JS代码, 而是会先进行一个预编译的过程, 根据上下文的不同, 预编译又可以分为:
- 全局预编译
- 函数预编译 每个执行上下文都有一个与之相关联的变量对象 (Variable Object, 简称 VO, 其实就是一个对象：{key : value}形式) , 当前执行上下文中所有的`变量`和`函数`都添加在其中。

### 全局预编译

1.创建一个GO对象
2.找变量声明，将变量声明作为GO的属性名，值为undefined
3.在全局找函数声明，将函数名作为GO对象的属性名，值赋予函数体
```js
global=100
function fn(){
	console.log(global);//undefined
	global=200
	console.log(global);//200
	var global=300
}
fn()
```

### 函数预编译

1.创建一个AO（Action Object）对象
2.找形参和变量声明，将变量声明和形参作为AO的属性名，值为undefined
3.将实参和形参统一
4.在函数体内找函数声明，将函数名作为AO对象的属性名，值赋予函数体
```js
function foo(a){
    var a=1
    var a=2
    function b(){}
    var b=a
    a=function c(){}
    console.log(a); //[Function: c]
    c=b
    console.log(c);//2
}
foo(2)

// AO:{
//     a:undefined 2 1 2 function c(){}
//     b:undefined function b(){} 2
//     c: function c(){} 2
// }
```

- 函数提升只针对具名函数，而对于赋值的匿名函数（表达式函数），并不会存在函数提升
- 提升优先级问题，函数提升优先级高于变量提升，且不会被同名变量声明覆盖，但是会被变量赋值后覆盖。而且存在同名函数与同名变量时，优先执行函数。

### 调用栈
调用栈用于跟踪函数的调用顺序和执行上下文的管理。每当函数被调用，一个新的执行上下文会被推入调用栈，表示该函数的执行。 全局预编译和函数预编译的结果在执行上下文中存储，然后被推入调用栈。调用栈的顶部始终包含当前正在执行的函数的上下文
![[Pasted image 20240503112820.png]]
全局执行上下文分为变量环境和词法环境，而我们用var声明的变量存在变量环境中。我们都知道栈是先进后出，为什么这里全局执行上下文在栈底部呢，也就是比fn执行上下文先进来，**因为在调用栈中，全局预编译通常会在函数预编译之前完成，因此全局预编译的结果位于调用栈的底部，而函数的预编译结果则根据函数调用的顺序依次位于调用栈中。**

![[Pasted image 20240503143653.png]]
  
### 作用域与执行上下文
许多开发人员经常混淆作用域和执行上下文的概念，误认为它们是相同的概念，但事实并非如此。
我们知道 JavaScript 属于解释型语言，JavaScript 的执行分为：解释和执行两个阶段,这两个阶段所做的事并不一样：
<mark class="hltr-cyan">解释阶段</mark>：
- 词法分析
- 语法分析
- 作用域规则确定
<mark class="hltr-cyan">执行阶段</mark>：
- 创建执行上下文
- 执行函数代码
- 垃圾回收
## 作用域

[JS入门理解——预编译、作用域、词法作用域和作用域链 - 掘金](https://juejin.cn/post/7117143748022108174?searchId=20231009190912A186E76B58762E2AFE67#heading-14)
[从JS执行过程彻底讲清楚闭包、作用域链、变量提升等 - 掘金](https://juejin.cn/post/7235463575300702263?searchId=20231009191245A01F9CB0B3237D4410E5#heading-10)
[js中的执行上下文、作用域、闭包和this - 掘金](https://juejin.cn/post/6962538808201969701?from=search-suggest#heading-7)
[JS中的作用域和作用域链 - 一粒一世界 - 博客园](https://www.cnblogs.com/leftJS/p/11067908.html)
[深入理解JS中声明提升、作用域（链）和\`this\`关键字 · Issue #16 · creeperyang/blog · GitHub](https://github.com/creeperyang/blog/issues/16)
### 全局作用域
全局作用域就是可以被全局访问和使用的全局变量。

优点是可以重复的使用，缺点是容易出现错误，变量容易出现被其他的地方窜改，代替的问题。
一般来说没有在函数的的大括号之内声明的变量一般都会是全局作用域范围的全局变量。比如在最外层声明的函数和变量,window对象内置的属性和方法也都是全局的，还有没有声明直接定义值的也会被自动定义为全局对象。

在程序顶部或函数外部声明的变量被认为是全局范围变量。全局作用域中声明的变量与函数，可以在代码的任何地方被访问。
<mark class="hltr-yellow">全局对象下的属性与方法</mark>
```js
window.name
window.location
window.top
```

<mark class="hltr-yellow">在最外层声明的变量与方法</mark>
```js
let name = "jimmy";
function greet() {
  console.log(name);
}
greet(); // jimmy
```

在非严格模式下，函数作用域中未定义但直接赋值的变量与方法
在非严格模式下，这样的变量自动变成全局对象window的属性。因此他们也是属于全局作用域。
```js
function foo() {
  bar = 20;
}
function fn() {
  foo();
  return bar + 30;
}
fn(); // 50
```
### 函数作用域
函数作用即局部作用域，外部无法访问到该作用域内的变量和方法，里面的变量也就是局部变量一般会随着外部函数的消失而被消失，被浏览器的垃圾回收机制所回收。函数作用域中的所声明的变量就是局部变量（包含的函数的参数）。优点是不会被污染，缺点是不可以重复使用。

每一个花括号 `{}` 都是一个代码块。但需要注意的是，并不是所有花括号，都能够具备自己的作用域。函数声明或者函数表达式，能够让花括号具备作用域，我们称之为函数作用域。函数作用域中声明的变量与方法，只能被下层子作用域访问，不能被其他不相关的作用域访问。
```js
let a = "hello";
function greet() {
    let b = "World"
    console.log(a + b);
}
greet();
console.log(a + b); // error

```
### 词法作用域
词法作用域（也叫静态作用域）从字面意义上看是说作用域在词法化阶段（通常是编译阶段）确定而非执行阶段确定的。 **JavaScript 的作用域是词法作用域。**
```js
let number = 42;
function printNumber() {
  console.log(number);
}
function log() {
  let number = 54;
  printNumber();
}
// Prints 42
log();
```
### 作用域链

相信前面的执行上下文部分同学们已经理解了，接下来我们会结合执行上下文来看**作用域链**：

- 每个执行上下文的变量环境中，都包含了一个外部引用，用来指向外部的执行上下文，我们把这个外部引用称为 outer。
- 当一段代码使用了一个变量的时候，js引擎会在当前执行上下文查找该变量，如果没有找到，会继续在outer执行的执行上下文中去寻找。这样一级一级的查找就形成了**作用域链**。作者：小猪皮皮呆  

当多重的作用域嵌套的时候就会产生一个由里向外的作用域链。
作用域链的规则是:js引擎从当前执行的作用域开始，先查看当前作用域是否存在，若如果不存在则继续向外寻找，直接找到最外层的全局作用域，才会停止查找。

![[Pasted image 20230827161130.png]]

### 块级作用域
**块级作用域由最近的一对包含花括号{} 界定。换句话说， if 块、while 块、function块，单独的块块级作用域。**
<mark class="hltr-yellow">let和cosnt</mark>实际上为 JavaScript 新增了块级作用域。
```js
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
ES6 允许块级作用域的任意嵌套。
内层作用域可以定义外层作用域的同名变量。
块级作用域的出现，实际上使得获得广泛应用的匿名立即执行函数表达式（匿名 IIFE）不再必要了。

- 声明变量不会提升到代码块顶部
- 禁止重复声明
- 暂时性死区
- 块级块级作用域

执行上下文角度
```js
function foo() {
    var a = 1
    let b = 2
    {
        let b = 3
        var c = 4
        let d = 5
        console.log(a)
        console.log(b)
    }
    console.log(b)
    console.log(c)
    console.log(d)
}
foo()

```

### 立即执行函数
[【JS】立即执行函数（IIFE）/函数声明/表达式解析 - 掘金](https://juejin.cn/post/6911252965877776392?searchId=202310091741530ACBA5CB7AEC421CF4E4#heading-9)
[进击的 JavaScript（五） 之 立即执行函数与闭包 - 掘金](https://juejin.cn/post/6844903638633037837?searchId=202310091741530ACBA5CB7AEC421CF4E4)
```js
//IIFE普通函数
(function add(x,y){ 
  console.log(x+y)
})(1,2);

//IIFE匿名函数
(function(x,y){ 
  console.log(x+y) 
}(1,2));


var funcs = []
for(var i = 0; i < 10; i++) {
    funcs.push(
        (function(value) {
		return function() {
			return value;
		}
	})(i)
    )
}
funcs.forEach(function(func) {
    console.log(func())
})
```
### 参数作用域
[函数参数默认值带来的作用域问题 - 掘金](https://juejin.cn/post/7041507111443890206?searchId=20231011192716F840063D7572599D4147)
**一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域（context）。等到初始化结束，这个作用域就会消失。这种语法行为，在不设置参数默认值时，是不会出现的**
```js
var x = 1;
function f(x, y = function () { x = 3; console.log(x); }) {
  console.log(x) // undefined
  var x = 2
  y() // 3
  console.log(x) // 2
}
f()
console.log(x) // 1
```

```js
var x = 1;
function f(x = 0,y = function () { x = 3; console.log(x); }) {
  console.log(x)
  var x = 2
  y()
  console.log(x)
}
f()
console.log(x)
0
3
2
1
```

```js
var x = 1;
function f(y,x) {
  console.log(x)
  var x = 2
  y()
  console.log(x)
}
f(function () { x = 3; console.log(x); })
console.log(x)
undefined
3
2
3
```
## this
[this，作用域与闭包 - 掘金](https://juejin.cn/post/7234058546572460092?searchId=20230827162543D07ADAF4DBFF1F04CA9E)
[Site Unreachable](https://github.com/creeperyang/blog/issues/16)
[理解this的上下文：JavaScript中的this指向机制 - 掘金](https://juejin.cn/post/7282150745162809400?searchId=20231009224509943F6ED5F332386BB1EA#heading-10)
先念口诀：箭头函数、new、bind、apply 和 call、欧比届点（obj.）、直接调用、不在函数里。
[JS 中 this 指向问题 - 掘金](https://juejin.cn/post/6946021671656488991?searchId=2023100922243589C3F65F40B1754FD38E#heading-4)

重点[看完就能搞懂的this指向及箭头函数的讲解～ - 掘金](https://juejin.cn/post/6844904145963466766?searchId=2023101120190525FE7FBD0C2EE2CBD619#heading-9)

**作用域和this之间没有任何关系**
### 全局作用域下的this
**this指向跟调用有关，跟定义无关**
例1
```js
function fun(){
    console.log(this.s);
}
//对象初始化时，会绑定this
var obj = {
    s:'1',
    f:fun
}

var s = '2';
//this指向跟调用有关，跟定义无关
obj.f(); //1
fun(); //2
```
例2
```js
var btn = document.getElementById('btn');
btn.onclick = function(){
    this ;  // this指向本节点对象
}
```
例3
```js
var obj = {
    fun:function(){
        this ;
    }
}

setInterval(obj.fun,1000);      // this指向window对象
setInterval('obj.fun()',1000);  // this指向obj对象
```

### 对象方法中(局部作用域下)的this

```js
let obj={
    name:'john',
    sayName(){console.log(this.name);}
}

obj.sayName();//john
```
**但是！！！**  
**对象的函数会进行 this 自动绑定，这并不代表函数的内部函数也会自动绑定 this**
```js
const obj2={
    name:"obj",
    fn:function () {
        consloe.log('fn1',this.name);//fn1 obj
        function fn2 () {
            consloe.log('fn2',this);//window
        }
        fn2();
    },
};
obj2.fn();

```
可以通过**箭头函数**来解决this指向问题。
```js
const obj2={
    name:"obj",
    fn:function () {
        consloe.log('fn1',this.name);//fn1 obj
        function fn2 = () => {
            consloe.log('fn2',this.name);//fn2 obj
        }
        fn2();
    },
};
obj2.fn();
```

### 箭头函数中的this
[【前端】js匿名函数和箭头函数的this - 掘金](https://juejin.cn/post/6844904066456223752?searchId=20231009224509943F6ED5F332386BB1EA)
在JS中，箭头函数是ES6新增的语法。与传统的函数不同，箭头函数中的this指向调用该函数的上下文环境（即**当前作用域所属的对象**）。  
例如，在如下代码段中，this指向obj：
```js
let obj={
    name:'jack',
    func1:function(){
        setTimeout(() => console.log(this.name),1000);
    }
}

obj.func1();//jack

```

箭头函数作为对象的方法，this指向window
```js
const obj = {
	name: 'ikun',
	foo: () => {
		console.log(this)
	}
}

obj.foo()

```

```js
var m = () => { console.log(this) };//定义了一个箭头函数为m
var obj = {
    a: m, // () => { console.log(this) }
    b: function () {
        m(); // 已经调用了
    },
    c: function () {
        var m2 = () => { console.log(this) };//每次调用都会新定义这个箭头函数
        m2();
    }
}
obj.a();//window
obj.b();//window
obj.c();//obj

var obj2={};
obj2.c=obj.c;
obj2.c();//obj2

```

**如果有需要更改函数内部this指向的需求，就不能使用箭头函数**  
**因为，用了箭头函数再用call，并不好发生this指向偏移**

### 构造函数中的this
在函数JS中，构造函数用于创建对象。每个构造函数都包含一个this关键字，它指向新构造的对象。  
例如，如下代码段中，this指向新创建的Person对象：
```js
const name="I am window"'

function Person(name){
    this.name=name;
    this.sayName = function () {
        consloe.log(this.name);//p1
        function fn(){
            console.log(this.name);//指向window的name
        }
        fn()//在没有在外部定义name时，打印为空;
            //定义name后，打印为：I am window
    };
}
//构造器函数，通过 new 关键字调用
const p1=new Person('P1');
p1.sayName();

```
this 在构造器中其实和在对象中类似。
### 借助函数实现 this 指向偏移
**call、apply、bind都可以改变this的指向**

<mark class="hltr-cyan">call()</mark>：将函数的this指定到call（）的第一个参数值，同时将剩余参数指定的情况下，调用某个函数或方法
```js
var obj1 = {
    a: 10,
    fn: function(x) {
        console.log(this.a + x)
    }
}
var obj2 = {
    a : 20,
    fn: function(x) {
        console.log(this.a - x)
    }
}
obj1.fn.call(obj2, 20) //40

`obj1.fn` 中 `fn`的`this`指向到 `obj2`，最后还是执行 `obj1.fn` 中的函数。
```

<mark class="hltr-cyan">apply()</mark>：和call基本一致，两者唯一的不同是：apply 的第一个参数还是调用函数this的指向，第二个参数是数组[arg1, arg2...]，call的第二参数是列表(arg1, arg2...)
```js
var name = 'AA'
var obj = {
    name: '林一一',
    fn: function() {
        return `${this.name + [...arguments]}`
    }
}
obj.fn.apply(window, [12, 23, 34, 56])    // "AA 12,23,34,56"

```

<mark class="hltr-cyan">bind()</mark>：bind()和 call、apply 的区别是 它不会立即执行，而是返回一个新的函数。**即bind()方法会创建一个新函数**。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。
```js
//这个新函数就是调用 bind 的函数，bind 调用后会将调用 bind 的函数拷贝一份返回。

var name = 'BD'
var obj = {
    name: '林一一'
}

function fn(){
    return `${this.name} ` + [...arguments]
}

let f = fn.bind(obj, 12, 23, 45, 67, 90)
f() // "林一一 12,23,45,67,90"
```

```js
<div onclick="say.call({name: 'he'})">111</div>
<div onclick="say.bind({name: 'he'})()">222</div>

<script>
function say(){
    console.log(this,"hello world");
}
</script>

```
正是因为bind不会立即执行，而是返回一个函数，所以在上述例子里与call的写法不同，因为如果不加上(),它就不会被调用。
##### 手写call
[[前端攻坚]：详解call、apply、bind的实现 - 掘金](https://juejin.cn/post/7170994543654305806?searchId=2023082717094933959781E3F830016237)

```js

_call(fn,...arg) = function(){
     if(obj===null||obj===undefined){ //没有指定对象时候
         obj =  window
     }
    obj.temp = fn //临时函数
    let res = obj.temp(...arg)
    delete obj.temp //临时函数完成它的任务就要消失了
    return res
}

//改进版
Function.prototype._call = function(obj,...arg){
    obj = obj || window //没有指定对象时候 指向window
    const temp = Symbol() //小技巧，Symbol可以用来声明一个对象的属性
    obj.temp = this //因为call的声明方法是`Function.prototype._call`所以this就是要调用的函数
    let res = obj.temp(...arg)
    delete obj.temp
    return res
}

```

##### apply
```js

Function.prototype.apply = function(obj,arg){
    obj = obj || window //没有指定对象时候 指向window
    const temp = Symbol() //小技巧，Symbol可以用来声明一个对象的属性
    obj.temp = this //因为call的声明方法是`Function.prototype._call`所以this就是要调用的函数
    let res = obj.temp(...arg)
    delete obj.temp
    return res
}

```

##### bind
```js
Function.prototype._bind = function (obj,...arg1) {
    obj = obj || window
    const temp = Symbol()
    obj[temp] = this
    return function(...arg2){
        obj[temp](...arg1,...arg2) //obj[temp](1,2,3,4)
    }
}

```


## 闭包
什么是闭包呢？我们先来看下官方解释。

> 1.小"黄"书(你不知道的JavaScript): 当函数可以记住并访问所在的词法作用域时,就产生了闭包,即使函数是在当前词法作用域之外执行.  
> 2.红宝书(JavaScript高级程序设计): 闭包是指有权访问另一个函数作用域中的变量的函数.  
> 3.MDN:闭包（closure）是一个函数以及其捆绑的周边环境状态（lexical environment，词法环境）的引用的组合。

```js
function f1(){
    var a=10
   function f2(){
      a++;
      console.log(a)    
    }
    return f2;
}
var retult= f1();
retult()//11
retult()//12
```
我们直接上案例，我们可以看到我们的变量不会随着函数执行结束而消失。可以一直被重复使用，那么什么是闭包呢，我的理解是外层的作用域中局部变量被里面的函数作用域链所牵引，不会随函数执行消失，从而产生闭包。

### 闭包的用途
闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。上面的代码就已经反映出来了闭包的两个作用。

### 闭包的缺陷
由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

```js
function f1(){
    function f2(){
        var a=10;
        a++;
        console.log(a)
    }   
    return f2;
}
var result=f1();
result();//11
result();//11

```


```js
var a = 2;
var obj = {
    a: 4,
    f1: (function () {
        this.a *= 2;
        var a = 3;
        return function () {
            this.a *= 2;
            a *= 3;
            console.log(a);
        };
    })(),
};
var f1 = obj.f1;



console.log(a);//4
f1();//9
obj.f1();//27

```

第一个a是4，因为我们f1是一个自执行函数，所以f1被执行this.a被2所以等于4，这个也是因为this指向window，所以调用的是全局的a，和上面一个题一样的道理。function的a没有，所以向外找，在f1中找到了a，停止寻找。因为调用的外部的函数中的变量所以不会随函数执行而消失，所以一直累乘，出现9和27.
### 隐藏数据提供api
```js
function createCache() {
    const data = {} // 闭包中的数据，被隐藏，不被外界访问
    return {
        set: function (key, val) {
            data[key] = val
        },
        get: function (key) {
            return data[key]
        }
    }
}

const c = createCache()
c.set('a', 100)
console.log( c.get('a') )
```
### 闭包作用题
```js
function test() {
    var n = 4399;
    function add() {
        n++;
        console.log(n);
    }
    return {
        n: n,
        add: add
    }
}
var result = test(); 
var result2 = test(); 
result.add();
result.add(); 
console.log(result.n); 
result2.add(); 

var result = test()  // 设置result的属性为： { n:4399, add: add() }
var result2 = test()  // 设置result2的属性为： { n:4399, add: add()  }result.add() // 调用了result的add方法，执行n++，所以n = 4399 + 1 输出4400
result.add() // 再次调用了result的add方法，此时n = 4400 所以n = 4400 + 1 输出4401
console.log(result.n)  //这是输出result的属性n值，输出4399,这里是输出对象属性返回值，并无调用闭包
result2.add() // 调用了result2的add方法，此时n = 4399 所以n = 4399 + 1 输出4400
```

```js
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log(this.foo);
		console.log(self.foo);
	(function() {
	    console.log(this.foo);
	    console.log(self.foo);
	}());
    }
};
myObject.func();

1、func由myObject调用，this指向myObject，输出bar
2、self 指向myObject,输出bar
3、立即执行匿名函数表达式由window调用，this指向window
4、立即执行执行函数在本作用域找不到self，沿着作用域链向上寻找self变量，找到了父函数中指向myObject对象的self
```

[JS 经典面试题初篇(this, 闭包, 原型...)含答案 - 掘金](https://juejin.cn/post/6943035836691087397?searchId=20231019175600379EC2DFE9EC8D7492E4)
```js
var num = 10    // 60； 65
var obj = {
    num: 20    
}
obj.fn = (function (num){
    this.num = num * 3
    num++    // 21
    return function(n){
        this.num += n    // 60 + 5 = 65；20 + 10 =30
        num++   // 21 + 1 = 22；22 + 1 = 23
        console.log(num)
    }
})(obj.num)
var fn = obj.fn
fn(5)   // 22
obj.fn(10)   // 23
console.log(num, obj.num)    // 65, 30

```
### 防抖 节流
[关于js防抖(闭包的一个经典应用) - 掘金](https://juejin.cn/post/7032539864377589790?searchId=20230828152942BA1BA667880174AD98E2)
[一文搞懂JS系列（五）之闭包应用-防抖，节流 - 掘金](https://juejin.cn/post/6885120726039265293?searchId=20230828152942BA1BA667880174AD98E2)
防抖
```js
function debounce(func,wait,...args) {
 let timeout;    //延时器变量
 return function () {
   const context = this;    //改变this指向
   if (timeout) clearTimeout(timeout);   //先判断有没有延时器，有则清空，毕竟要最后一次执行
   timeout = setTimeout(() => {      
     func.apply(context, args)     //apply调用传入方法
   }, wait);
 }
}

```

节流
```js
// 时间戳
function throttle(func, wait, ...args){
   let pre = 0;
   return function(){
       const context = this;
       let now = Date.now();
       if (now - pre >= wait){
           func.apply(context, args);
           pre = Date.now();
       }
   }
}

```

```js
// 延时器

```