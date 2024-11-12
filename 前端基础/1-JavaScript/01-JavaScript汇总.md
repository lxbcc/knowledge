[分类：JavaScript - 阮一峰的网络日志](https://www.ruanyifeng.com/blog/javascript/)
## 数据类型

### JS 内存和其他语言的差别

在其他常见的语言中，用来保存变量和对象的内存一般分为栈内存和堆内存，而在 JavaScript 中，所谓的栈和堆都是放在堆内存中的，而在堆内存中，js 把其分为栈结构和堆常被误认为是堆内存和栈内存，但是我们可以把它简称为栈和堆。
![[Pasted image 20230502200901.png|550]]

### 栈和堆的特点和区别
**栈 (stack)**: 栈会自动分配内存空间，会自动释放，存放 <mark class="hltr-cyan">基本数据类型</mark> 和 <mark class="hltr-cyan">引用类型</mark> 的变量。
所有在函数/方法中定义的变量都是放在栈中，随着函数/方法的执行结束，这个函数/方法的内存栈也自然销毁。
优点：存取速度比堆快。
缺点：存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。
**堆(heap)** :动态分配的内存，大小不定也不会自动释放，存放**引用类型的对象**，指那些可能由多个值构成的对象，保存在堆内存中。
堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用(参数传递)。创建对象是为了反复利用。

### 分类

**基本类型**：字符串（String）、数字(Number)、布尔(Boolean)、空（Null）、未定义（Undefined）、Symbol（独一无二的值）、BigInt
**引用类型**：对象(Object)、数组(Array)、函数(Function)
### Symbol

[js symbol类型详解以及symbol的三大应用场景-CSDN博客](https://blog.csdn.net/darabiuz/article/details/121962153)
[Symbol是JavaScript中的原始数据类型之一 - 掘金](https://juejin.cn/post/7226193000496463928?searchId=202310191657037D31CE7E4C31FF715111)
[那些小而美的JS基础知识 - Symbol - 掘金](https://juejin.cn/post/7281574972208136226?searchId=202310191657037D31CE7E4C31FF715111)

```js
let a = symbol(); 
let b = symbol("foo"); 
```

**特点**
- symbol可以转化为字符串、布尔值；转化为其他类型均报错。
- symbol不能参与计算。
- 因为symbol值是独一无二的，所以，即使描述符相同的2个symbol值是不相等的。
- symbol值作为属性名字时，<mark class="hltr-yellow">不能使用点运算符来得到该属性</mark>，而是使用方括号的形式来get or set。因为点运算符后面总是跟着字符串。
- symbol类型的对象属性名字，不能被 Object.keys、Object.getOwnPropertyNames、for...in...、for...of... 遍历到。
```js
let b1 = 'b1';

let obj1 = {
    a: 11,
    b: 22,
    [Symbol('sty')]: 55,
    [b1]: 66
}
obj1.__proto__ = {
    c: 33
}
Object.keys(obj1);                  // 【“a”，“b”，“b1”】
Object.getOwnPropertyNames(obj1);   // 【“a”，“b”，“c1”】
Object.getOwnPropertySymbols(obj1); // 【Symbol(sty)】
```

**如何区分Symbol?**  
在通过Symbol生成独一无二的值时可以设置一个标记  
这个标记仅仅用于区分, 没有其它任何含义

使用Symbol类型作为属性名
默认是字符串，所以不加‘’也可以，如果需要类型为symbol，需要使用[]
不可以用`.`来访问,因为点运算符后面总是字符串
Symbol 值作为属性名时，该属性还是公开属性，不是私有属性。
```js
//后面的括号可以给symbol做上标记便于识别
let name=Symbol('name');
let say=Symbol('say');
let obj= {
  //如果想 使用变量作为对象属性的名称，必须加上中括号，.运算符后面跟着的都是字符串
  [name]: 'lnj',
  [say]: function () {
    console.log('say')
  }
}
obj.name='it666';
obj[Symbol('name')]='it666'
console.log(obj)

```
### BigInt
[JavaScript基础类型BigInt简介](https://baijiahao.baidu.com/s?id=1681408982854883134&wfr=spider&for=pc)

是一种特殊的数字类型，它支持任意长度的整数。
```js
let n = 520n;//他只用在普通整型后边添加一个n就可以了
console.log(n,typeof(n))

const sameBigint = BigInt("1234567890123456789012345678901234567890");

//不能使用浮点数进行转换
console.log(BigInt(0.2));

let max = Number.MAX_SAFE_INTEGER;//Number的最大安全整数
console.log(max);
console.log(max + 1);
//超过number的最大数值范围，运算就会出错
console.log(max + 2);

console.log(BigInt(max));
//BigInt数据类型不能直接和普通数据类型进行运算
console.log(BigInt(max) + BigInt(1)); // 9007199254740992n
console.log(BigInt(max) + BigInt(2)); // 9007199254740993n
```

### 基础类型和引用类型区别

1. 存储位置不同
   **基本数据类型** ：以栈的形式存储，保存与赋值指向数据本身，用 typeof 来判断类型, 存储空间固定。
   **引用数据类型** ：存放在堆内存中的对象，变量其实是保存的在栈内存中的一个指针（保存的是堆内存中的引用地址），这个指针指向堆内存。用 **instanceof** 来判断类型。

[数据类型-掘金]( https://juejin.cn/post/7021908477475815455 )

2. 传值方式不同
   基本数据类型按值传递，无法改变一个基本数据类型的值
   引用类型按引用传递，引用类型值可以改变

### null，undefined 或 undeclared
#null 表示"没有对象"，即该处不应该有值，转为数值时为0。典型用法是：
（1） 作为函数的参数，表示该函数的参数不是对象。
（2） 作为对象原型链的终点。
#undefined 表示"缺少值"，就是此处应该有一个值，但是还没有定义，转为数值时为 NaN。典型用法是：
（1）变量被声明了，但没有赋值时，就等于undefined。
（2) 调用函数时，应该提供的参数没有提供，该参数等于undefined。
（3）对象没有赋值的属性，该属性的值为undefined。
（4）函数没有返回值时，默认返回 undefined。
#undeclared : js 语法错误，没有申明直接使用，js 无法找到对应的上下文。


## 判断 js 类型的方法
["你真的了解 JavaScript 类型检测吗？深入探讨 typeof、instanceof" - 掘金](https://juejin.cn/post/7343138527419220003?searchId=202405071120090A03EAB1EA5D74740DFC)
### typeof
可以判断出'string','number','boolean','undefined','symbol'  
但判断 typeof(null) 时值为 'object'; 判断数组和对象时值均为 'object'
### instanceof
<mark class="hltr-cyan">instanceof</mark> 操作符在JavaScript中用于检查一个对象是否是某个特定构造函数的实例

object instanceof constructor

<mark class="hltr-cyan">基本用法</mark>
```js
function Person(name) {
    this.name = name;
}
const person = new Person("Alice");
console.log(person instanceof Person); // true
```

<mark class="hltr-cyan">继承关系</mark>
```js
class Animal {}
class Dog extends Animal {}

const myDog = new Dog();

console.log(myDog instanceof Dog);      // true
console.log(myDog instanceof Animal);   // true
console.log(myDog instanceof Object);   // true
```
<mark class="hltr-cyan">myDog</mark>是<mark class="hltr-cyan">Dog</mark>的实例，同时由于JavaScript的原型继承机制，Dog继承自<mark class="hltr-cyan">Animal</mark>，因此myDog也是Animal的实例。同理，所有对象默认继承自<mark class="hltr-cyan">Object</mark>，所以myDog instanceof Object也返回true。

<mark class="hltr-cyan">注意事项</mark>
- instanceof 检查的是对象的原型链，因此它不能用于基本数据类型（如字符串、数字和布尔值），因为它们不是对象。console.log('123' instanceof String) // false
- 如果一个对象是通过不同的全局执行上下文中的相同函数构造的，则`instanceof`可能返回`false`，因为每个全局执行上下文都有自己的一套原型链

### Object .prototype.toString.call()
[js中toString方法3个作用\_javascript技巧\_脚本之家](https://www.jb51.net/article/233178.htm)

<mark class="hltr-cyan">Object.prototype.toString.call()</mark>是一种利用`Object`原型链上的`toString`方法，通过`call`或`apply`方法改变`this`上下文来检测对象类型的技巧

这种方法能够返回一个明确标识对象类型的字符串，格式为`"[object Type]"`，其中`Type`是被检测对象的类型。
这种方式的优势在于它能提供比`typeof`更细致的类型信息，能够区分各种引用类型，如数组、日期、正则表达式等。

- `({}).toString()`直接调用空对象的`toString`方法，通常返回`"[object Object]"`，表示这是一个普通对象。

- `([]).toString()`直接调用空数组的`toString`方法，返回空字符串`""`，因为数组没有元素。

- `Object.prototype.toString.call([])`使用`call`方法将`Object.prototype.toString`应用于数组对象，返回`"[object Array]"`，准确地表示这是一个数组对象。

- `Object.prototype.toString.call({})`同样使用`call`方法，但作用于一个普通对象上，返回`"[object Object]"`，表示这是一个普通对象。


```js
Object.prototype.toString.call(null)//"[object Null]"
Object.prototype.toString.call(undefined)//"[object Undefined]"
Object.prototype.toString.call(Object)//"[object Function]"
```
### Array.isArray()
用于判断是否为数组。
```js
console.log(Array.isArray([]));        // true
console.log(Array.isArray({}));        // false
console.log(Array.isArray('Hello'));   // false
```

## for..in 和 object.keys 的区别

Object.keys 不会遍历继承的原型属性
for...in 会遍历继承的原型属性
## JS 中的匿名函数是什么

匿名函数：就是没有函数名的函数
```js
(function(x,y){
	alert(x+y)
})(2,3)
```
这里创建了一个匿名函数(在第一个括号内)，第二个括号用于调用该匿名函数，并传入参数。

## Object.is、 == 、 === 

== 会在比较时进行类型转换
=== 比较时不进行隐式类型转换，类型不同则会返回 false
### JS 中 == 和 === 区别是什么？

<mark class="hltr-green">对于 String Number 等基础数据类型</mark>，有区别
1. 不同类型间比较，会进行数据 <mark class="hltr-cyan">类型转化</mark> , 将二者转换成数据类型相同的变量，再进行比较
2. 同类型比较，直接进行“值”比较，两者结果一样。
```js
NaN == NaN false NaN和任何数都不相等，包括NaN本身
undefined == null true 但是 undefined === true false (因为数据类型不一样)。
```

<mark class="hltr-green">对于 Array,Object 等高级类型</mark>，== 和 === 没有区别 进行“指针地址”比较。

<mark class="hltr-green">基础类型与高级类型</mark>，== 和 === 有区别
== 将高级类型转换为基础类型，进行值比较
=== 直接 false，类型不同
== 数据类型转换

<mark class="hltr-cyan">基本类型</mark>
基本类型的数据会转换成<mark class="hltr-yellow">数值类型</mark>再进行比较。
```js
1 == true // true
// 等同于1 === number(true)
0 == false // true
// 等同于 0 === Number(false)
2 == true // false
2 == false // false
'true' == true // flase
// 等同于Number('true') === Number(true)
// 等同于 NaN === 1
'' == 0 // true
// Number('')  === 0 
'' == false // true
'1' == true // true
'123' == true // false
```

<mark class="hltr-cyan">引用类型</mark>
引用数据类型 转化成原始类型的值，再进行比较
```
[1] == 1 // true
// 等同于 Number([1]) == 1

[1] == '1' // true
// 等同于 Number([1]) == Number('1')

[1] == true // true
// 等同于 Number([1]) == Number(true)

Number({}) // NaN
```

<mark class="hltr-cyan">undefined 和 null</mark>
undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。
```
Number(null) // 0
Number(undefined) // NaN
undefined == null // true
```
## 事件委托

<mark class="hltr-cyan">含义</mark>：<mark class="hltr-yellow">事件代理/事件委托是利用事件冒泡的特性，将本应该绑定在多个元素上的事件绑定在他们的祖先元素上</mark>
### 为什么要用事件委托？

在JavaScript中，添加到页面上的事件处理程序数量将直接关系到页面的整体运行性能，因为需要不断地与dom节点进行交互，访问dom的次数越多，浏览器重绘与重排的次数也就越多
如果要用事件委托，就会将所有的操作放到js程序里面，与dom的操作就只需要交互一次，这样就能大大的减少与dom的交互次数，提高性能；
(1)使代码更简洁；(2)节省内存开销

<mark class="hltr-cyan">事件冒泡</mark>：
事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发

<mark class="hltr-cyan">事件捕获</mark>：
事件从最不精确的对象(document 对象)开始触发，然后到最精确(也可以在窗口级别捕获事件，不过必须由开发人员特别指定)。与事件冒泡相反，事件会从最外层开始发生，直到最具体的元素

<mark class="hltr-cyan">阻止事件冒泡</mark>
w3c的方法是，e.stopPropagition(),IE则是使用e.cancelBubble = true
window.event.cancelBubble = true；
e.stopPropagation()
return false也可以阻止冒泡。

<mark class="hltr-cyan">阻止默认事件</mark>
w3c的方法是e.preventDefault()，IE则是使用e.returnValue = false，比如：
``` js
function stopDefault(e) {
	if (e && e.preventDefault){
		e.preventDefault()
	} else {
		window.event.returnValue = false;
	}
} 
```
return false也能阻止默认行为。

DOM 事件有哪些阶段？谈谈对事件代理的理解
分为三大阶段：捕获阶段--目标阶段--冒泡阶段

### 响应事件有哪些？
 鼠标单击事件（ onclick ）
 鼠标经过事件（ onmouseover ）
 鼠标移开事件（ onmouseout ）
 光标聚焦事件（ onfocus ）
 失焦事件（ onblur ）
 内容选中事件（ onselect ）
 文本框内容改变事件（ onchange ）
 加载事件（ onload ）:onload 只能在body中使用，一旦完全加载所有内容（包括图像、脚本文件、CSS 文件等），就执行一段脚本。
 卸载事件（ onunload ）


## 对象

### new操作符
[new](https://blog.csdn.net/qq_44717274/article/details/120135243)
https://www.cnblogs.com/echolun/p/10903290.html

在js中，new的作用是通过构造函数来创建一个实例对象
1. 创建一个空的对象，例如obj
2. 将空对象原型的内存地址指向__proto__ 指向函数的原型对象
3. 利用函数的call方法，将原本指向window的绑定对象this指向了obj
4. 利用函数返回对象obj
```js
function Foo(name) { 
	this.name = name; return this; 
} 
var obj = {}; 
obj.__proto__ = Foo.prototype; // Foo.call(obj, 'mm');
var foo = Foo.call(obj, 'mm');
console.log(foo);
```

<mark class="hltr-cyan">分析：</mark>
obj指向空对象
obj的原型地址指向构造函数Foo的原型对象
执行`Foo.call(obj, 'mm');`
	this.name = name 通过函数的call方法将this绑定到obj，实参mm传入构造函数Foo中，这样this.name = 'mm'，那么obj.name = 'mm'，也就是说name属性被挂载到obj对象上。
`return this;` 就是return obj，这样obj这个对象就被返回出来了
闭包
闭包的概念：闭包就是能读取其他函数内部变量的函数




### this指向
this永远指向函数运行时所在的对象，而不是函数被创建时所在的对象。

普通的函数调用，函数被谁调用，this就是谁。

构造函数的话，如果不用new操作符而直接调用，那即this指向window。用new操作符生成对象实例后，this就指向了新生成的对象。

匿名函数或不处于任何对象中的函数指向window 。

如果是call，apply等，指定的this是谁，就是谁。

call apply bind
call apply bind都可以改变函数调用的this指向。
```js
函数.call(对象,arg1,arg2...)
函数.apply(对象,[arg1,arg2...])
var ss = 函数.bind(对象,arg1,arg2...)
```

## 继承
[js的六种继承方式](https://blog.csdn.net/weixin_41759744/article/details/125299029)

### 1. 原型链继承
	核心：将父类的实例作为子类的原型
```js
function Parent1() {
	this.name = 'parent1'
	this.play = [1, 2, 3]
}
function Child1() {
	this.type = 'child'
}
Child1.prototype = new Parent1()
// 潜在的问题
let  s1  = new Child1()
let  s2  = new Child1()
s1.play.push(4)
console.log(s1.play, s2.play); // [1,2,3,4] [1,2,3,4]
// 两个实例使用的是同一个原型对象，他们的内存空间是共享的，当一个发生变化时，另一个也随之进行了变化，
```
特点： 
- 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
- 父类新增原型方法/原型属性，子类都能访问到
- 简单，易于实现
缺点： 
- 要想为子类新增属性和方法，必须要在new Animal() 这样的语句之后执行，不能放到构造器中
- 无法实现多继承
- 来自原型对象的所有属性被所有实例共享
### 2. 构造函数继承（call）
核心：使用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
```js
  function Parent1(){
    this.name = 'parent1';
  }
 
  Parent1.prototype.getName = function () {
    return this.name;
  }
 
  function Child1(){
    Parent1.call(this);
    this.type = 'child1'
  }
 
  let child = new Child1();
  console.log(child);  // 没问题
  console.log(child.getName());  // 会报错
```
特点：
- 解决了原型链继承,子类实例共享父类引用属性的问题
- 创建子类实例时，可以向父类传递参数
- 可以实现多继承（call多个父类对象）
缺点： 
- 实例并不是父类的实例，只是子类的实例
- 只能继承父类的实例属性和方法，不能继承原型属性/方法
- 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能
### 3. 组合继承（原型链继承+ 构造函数继承）
特点：通过调用父类构造
```js
  function Parent3 () {
    this.name = 'parent3';
    this.play = [1, 2, 3];
  }
 
  Parent3.prototype.getName = function () {
    return this.name;
  }
  function Child3() {
    // 第二次调用 Parent3()
    Parent3.call(this);
    this.type = 'child3';
  }
 
  // 第一次调用 Parent3()
  Child3.prototype = new Parent3();
  // 手动挂上构造器，指向自己的构造函数
  Child3.prototype.constructor = Child3;
  var s3 = new Child3();
  var s4 = new Child3();
  s3.play.push(4);
  console.log(s3.play, s4.play);  // 不互相影响
  console.log(s3.getName()); // 正常输出'parent3'
  console.log(s4.getName()); // 正常输出'parent3'

```
特点：
- 弥补了构造函数继承的缺陷，可以继承实例属性、方法，也可以继承原型属性方法
- 既是子类的实例也是父类的实例
- 不存在引用属性共享的问题
- 可传参
- 函数可复用
缺点：
- 调用了两次父类构造函数，生成了两份实例
### 4. 原型式继承
ES5 里面的 Object.create 方法，这个方法接收两个参数：一是用作新对象原型的对象、二是为新对象定义额外属性的对象（可选参数）
```js
  let parent4 = {
    name: "parent4",
    friends: ["p1", "p2", "p3"],
    getName: function() {
      return this.name;
    }
  };
 
  let person4 = Object.create(parent4);
  person4.name = "tom";
  person4.friends.push("jerry");
  let person5 = Object.create(parent4);
  person5.friends.push("lucy");
 
  console.log(person4.name); // tom
  console.log(person4.name === person4.getName()); // true
  console.log(person5.name); // parent4
  console.log(person4.friends); // ['p1', 'p2', 'p3', 'jerry', 'lucy']
  console.log(person5.friends); // ['p1', 'p2', 'p3', 'jerry', 'lucy']

```
- 通过 Object.create 这个方法可以实现普通对象的继承，不仅仅能继承属性，同样也可以继承 getName 的方法。
- 第一个结果“tom”，比较容易理解，person4 继承了 parent4 的 name 属性，但是在这个基础上又进行了自定义。
- 第二个是继承过来的 getName 方法检查自己的 name 是否和属性里面的值一样，答案是 true。
- 第三个结果“parent4”也比较容易理解，person5 继承了 parent4 的 name 属性，没有进行覆盖，因此输出父对象的属性。
- 最后两个输出结果是一样，其实 Object.create 方法是可以为一些对象实现浅拷贝的。
5. 寄生继承
使用原型式继承可以获得一份目标对象的浅拷贝，然后利用这个浅拷贝的能力再进行增强，添加一些方法，这样的继承方式就叫作寄生式继承。
```js
let parent5 = {
    name: "parent5",
    friends: ["p1", "p2", "p3"],
    getName: function() {
      return this.name;
    }
  };
 
function clone(original) {
    let clone = Object.create(original);
    clone.getFriends = function() {
      return this.friends
    };
    return clone;
}
 
  let person5 = clone(parent5);
  console.log(person5.getName()); // parent5
  console.log(person5.getFriends()); //  ['p1', 'p2', 'p3']

```
### 6. 组合寄生继承
结合第四种中提及的继承方式，解决普通对象的继承问题的 Object.create 方法，我们在前面这几种继承方式的优缺点基础上进行改造，得出了寄生组合式的继承方式，这也是所有继承方式里面相对最优的继承方式。
```js
  function clone (parent, child) {
    // 这里改用 Object.create 就可以减少组合继承中多进行一次构造的过程
    child.prototype = Object.create(parent.prototype);
    child.prototype.constructor = child;
  }
 
  function Parent6() {
    this.name = 'parent6';
    this.play = [1, 2, 3];
  }
   Parent6.prototype.getName = function () {
    return this.name;
  }
  function Child6() {
    Parent6.call(this);
    this.friends = 'child5';
  }
 
  clone(Parent6, Child6);
 
  Child6.prototype.getFriends = function () {
    return this.friends;
  }
 
  let person6 = new Child6();
  console.log(person6);  // child6 {name: "parent6",play: [1, 2, 3], friends: "child5"}
  console.log(person6.getName()); // parent6
  console.log(person6.getFriends()); // child5

```
![[Pasted image 20230511194657.png]]

函数声明和函数表达式的区别
在JavaScript中，解析器在向执行环境中加载数据时，对函数声明和函数表达式并非是一视同仁的，解析器会先读取函数声明，并使其在执行任何代码之前可用，至于函数表达式，则必须等到解析器执行到他所在的代码行，才会真正被解析

什么是window对象，什么是document对象
window对象代表浏览器中打开的一个窗口，document对象代表整个html文档，实际上，document对象是window对象的一个属性

document.onload和documen.ready
页面加载完成有两种事件，一是ready，表示文档结构已经加载完成（不包含图片等非文字媒体文件），二是onload，指示页面包含图片等文件在内的所有元素都加载完成。


