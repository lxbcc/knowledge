### let命令
#### 基本用法
ES6新增了`let`命令，用来声明变量他的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效
```js
{
	let a = 10
	var b = 1
}
a // ReferenceError: a is not defined.
b // 1
```
上面代码在代码块之中，分别用`let`和`var`声明了两个变量。然后在代码块之外调用这两个变量，结果`let`声明的变量报错，`var`声明的变量返回了正确的值。这表明，`let`声明的变量只在它所在的代码块有效。

`for`循环的计数器，就很合适使用`let`命令。

```javascript
for (let i = 0; i < 10; i++) {
  // ...
}

console.log(i);
// ReferenceError: i is not defined
```

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
上面代码中，变量`i`是`var`命令声明的，在全局范围内都有效，所以全局只有一个变量`i`。每一次循环，变量`i`的值都会发生改变，而循环内被赋给数组`a`的函数内部的`console.log(i)`，里面的`i`指向的就是全局的`i`。也就是说，所有数组`a`的成员里面的`i`，指向的都是同一个`i`，导致运行时输出的是最后一轮的`i`的值，也就是 10。
```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

上面代码中，变量`i`是`let`声明的，当前的`i`只在本轮循环有效，所以每一次循环的`i`其实都是一个新的变量，所以最后输出的是`6`。你可能会问，如果每一轮循环的变量`i`都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量`i`时，就在上一轮循环的基础上进行计算。

另外，`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++) { // 父作用域
  let i = 'abc'; // 子作用域。
  console.log(i);
}
// abc
// abc
// abc
```
---
#### 不存在变量提升
`var`命令会发生“变量提升”现象，即变量可以在声明之前使用，值为`undefined`。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这种现象，`let`命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
---
#### 暂时性死区
只要块级作用域内存在let命令，它所声明的变量就绑定这个区域，不再受外部的影响
```js
var temp = 123
if (true) {
	temp = 'abc'
	let temp
}
```
上面代码中，存在全局变量temp，但是块级作用域内let又声明了一个局部变量temp，导致后者绑定这个块级作用域，所以在let声明变量前对temp赋值会报错
ES6明确规定如果区块中存在let 和 const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域，凡是在声明之前就使用这些变量就会报错
总之，在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”

“暂时性死区”也意味着`typeof`不再是一个百分之百安全的操作。
```javascript
typeof x; // ReferenceError
let x;
```
上面代码中，变量`x`使用`let`命令声明，所以在声明之前，都属于`x`的“死区”，只要用到该变量就会报错。因此，`typeof`运行时就会抛出一个`ReferenceError`。

作为比较，如果一个变量根本没有被声明，tpeof反而不会报错

```javascript
typeof undeclared_variable // "undefined"
```
上面代码中，`undeclared_variable`是一个不存在的变量名，结果返回“undefined”。所以，在没有`let`之前，`typeof`运算符是百分百安全的，永远不会报错，


比较隐蔽的死区
```js
function bar(x=y, y=2){
	return [x=y]
}
bar() // 报错
```
参数`x`默认值等于另一个参数`y`,此时y还没有声明,属于死区，如果`y`的默认值是`x`，就不会报错，因为此时`x`已经声明了。

```js
var x = x // 不报错
let x = x // 报错
// ReferenceError: x is not defined
```
---
#### 不允许重复声明
`let`不允许在相同作用域内，重复声明同一个变量。
```javascript
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```
不能在函数内部重新声明参数。
```javascript
function func(arg) {
  let arg;
}
func() // 报错

function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```
---

### 块级作用域
#### 为什么需要块级作用域
es5只有全局作用域和函数作用域，没有块级作用域
1. 内层变量可能会覆盖外层变量
```js
var tmp = new Date()
function f() {
	console.log(tmp)
	if(false) {
		var tmp = 'hello world' 
	}
}
```
上面代码的原意是，`if`代码块的外部使用外层的`tmp`变量，内部使用内层的`tmp`变量。但是，函数`f`执行后，输出结果为`undefined`，原因在于变量提升，导致内层的`tmp`变量覆盖了外层的`tmp`变量。
2. 用来计数的循环变量泄露为全局变量
```javascript
var s = 'hello';
for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
```
#### ES6的块级作用域
let为js新增了块级做作用域
ES6 允许块级作用域的任意嵌套。
```javascript
{{{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}}}};
```
上面代码使用了一个五层的块级作用域，每一层都是一个单独的作用域。第四层作用域无法读取第五层作用域的内部变量。

块级作用域的出现，实际上使得获得广泛应用的匿名立即执行函数表达式（匿名 IIFE）不再必要了。
```javascript
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```
#### 块级作用域与函数声明

ES6引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。
```javascript
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
```
上面代码在 ES5 中运行，会得到“I am inside!”，因为在`if`内声明的函数`f`会被提升到函数头部，实际运行的代码如下。
```javascript
// ES5 环境
function f() { console.log('I am outside!'); }

(function () {
  function f() { console.log('I am inside!'); }
  if (false) {
  }
  f();
}());
```
ES6 就完全不一样了，理论上会得到“I am outside!”。因为块级作用域内声明的函数类似于`let`，对作用域之外没有影响。但是，如果你真的在 ES6 浏览器中运行一下上面的代码，是会报错的
```javascript
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```
ES6 在[附录 B](https://www.ecma-international.org/ecma-262/6.0/index.html#sec-block-level-function-declarations-web-legacy-compatibility-semantics)里面规定，浏览器的实现可以不遵守上面的规定，有自己的[行为方式](https://stackoverflow.com/questions/31419897/what-are-the-precise-semantics-of-block-level-functions-in-es6)。

- 允许在块级作用域内声明函数。
- 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。
- 同时，函数声明还会提升到所在的块级作用域的头部。
注意，上面三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作`let`处理。
根据这三条规则，浏览器的 ES6 环境中，块级作用域内声明的函数，行为类似于`var`声明的变量。上面的例子实际运行的代码如下。

```javascript
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }
(function () {
  var f = undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }

  f();
}());
// Uncaught TypeError: f is not a function
```
考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

```javascript
// 块级作用域内部的函数声明语句，建议不要使用
{
  let a = 'secret';
  function f() {
    return a;
  }
}

// 块级作用域内部，优先使用函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```
另外，还有一个需要注意的地方。ES6 的块级作用域必须有大括号，如果没有大括号，JavaScript 引擎就认为不存在块级作用域。

```javascript
// 第一种写法，报错
if (true) let x = 1;

// 第二种写法，不报错
if (true) {
  let x = 1;
}
```
上面代码中，第一种写法没有大括号，所以不存在块级作用域，而`let`只能出现在当前作用域的顶层，所以报错。第二种写法有大括号，所以块级作用域成立。

函数声明也是如此，严格模式下，函数只能声明在当前作用域的顶层。
```javascript
// 不报错
'use strict';
if (true) {
  function f() {}
}

// 报错
'use strict';
if (true)
  function f() {}
```
---
### const命令
#### 基本用法
`const`声明一个只读的常量，一旦声明，常量的值就不能改变。
`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值。
对于`const`来说，只声明不赋值，就会报错。
`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。
`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。
```javascript
if (true) {
  console.log(MAX); // ReferenceError
  const MAX = 5;
}
```
`const`声明的常量，也与`let`一样不可重复声明。