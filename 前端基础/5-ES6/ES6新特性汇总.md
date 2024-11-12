[地址链接](https://www.cnblogs.com/ruhaoren/p/11575179.html)
## 一、新的原始类型和变量声明
### symbol
在ES6之前，我们知道JavaScript支持6种数据类型：object，string，boolean，number，null，undefined。现在，ES6新增了一种原始数据类型：symbol，表示独一无二的值，即每个symbol类型的值都不相同
```js
var sy = Symbol('test')
var sy1 = Symbol('test')
console.log(tepeof sy);//'symbol'
sy == sy1;//false
var sy2 = new Symbol('test');//error : Symbol is not a constructor
```
创建symbol数据类型的值时，需要给Symbol函数传递一个字符串，并且有一点特殊的是：不能使用new关键字调用它。另外，每个symbol类型值都是独一无二的，即使传递的是相同的字符串。

### let 和 const
ES6新增了两个声明变量的关键字：let和const。
`块级作用域起作用`
他们声明的变量仅在let和const关键字所在的代码块内起作用，即在使用let和const的那一对大括号{}内起作用，也称块级作用域（ES6之前只有函数作用域和全局作用域）。
`不会变量提升`
let和const声明变量不会在预编译过程中有提升行为(在全局声明也不会变成window的属性)，且同一变量不能重复声明。所以要使用这类变量，只能在let和const关键字之后使用它们。
`暂时性死区`
let和const关键字还有一个特性：“暂时性死区”，即在使用了该关键字的块级作用域中，其内部使用let和const关键字声明的变量与外部作用域中的变量相互隔绝，互不影响。即使是同名变量。


const用来声明一个常量，声明时必须赋值，且一旦声明就不能改变。(`不准确`)
const声明的如果是一个原始值，那么上面的说法是准确的，如果const声明的是一个引用值，那么更准确的说法应该是*一个不能被重新赋值的变量*。

### 解构赋值
解构赋值是对赋值运算符的扩展。它是一种针对数组或者对象进行模式匹配，然后对其中的变量进行赋值。
`数组的解构赋值`
```js
let [a,b,c] = [1,2,3]
console.log(a,b,c) // 1,2,3

let [a,b,c] = [1,,3];
console.log(a,b,c);//1,undefined,3

let [a,,b] = [1,2,3];
console.log(a,b);//1,3

let [a,..b] = [1,2,3];//...是剩余运算符，表示赋值运算符右边除第一个值外剩余的都赋值给b
console.log(a,b);//1,[2,3]

```
解构不成功，变量的值就等于`undefined`。
```javascript
let [foo] = [];
let [bar, foo] = [1];
```
以上两种情况都属于解构不成功，`foo`的值都会等于`undefined`。

另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。

```javascript
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```
`默认值`
解构赋值允许指定默认值。
```javascript
let [foo = true] = [];
foo // true

let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
```
注意，ES6内部使用严格相等运算符=== ，判断一个位置是否有值，所以只有当一个数组成员严格等于undefined，默认值才生效。
```javascript
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

上面代码中，如果一个数组成员是`null`，默认值就不会生效，因为`null`不严格等于`undefined`。


事实上所有可枚举（iterable）的对象都可以使用解构赋值，例如数组，字符串对象，以及ES6新增的Map和Set类型。
```js
let arr = 'hello';
let [a,b,c,d,e] = arr;
console.log(a,b,c,d,e);//'h','e','l','l','o'
```
对象的解构赋值和数组类似，不过左边的变量名需要使用对象的属性名，并且用大括号{}而非中括号[]：
```js
let obj = {name:'ren',age:12,sex:'male'};
let { name,age,sex } = obj
console.log(name,age,sex) //'ren' 12 'male'

let {name:myName,age:myAge,sex:mySex} = obj;//自定义变量名
console.log(myName,myAge,mySex);//'ren' 12 'male'
```
### for...of...
`for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、 Generator 对象，以及字符串。
### 模板字符串
```js
// es5
let html1 = "我的名字是" + name + ", 我今年" + age + "岁。";
// es6
let html2 = `我的名字是${name}, 我今年${age}岁。`
```
### rest 运算符
也是三个点号（形式为...变量名），不过其功能与扩展运算符恰好相反，把逗号隔开的散列组合成一个数组，用于**获取函数的多余参数**，这样就不需要使用 arguments 对象。
```js
 function fun(a, b, ...values) {
      console.log(a); //1
      console.log(b); //2
      console.log(values); //[3, 4, 5, 6]
    }
    fun(1, 2, 3, 4, 5, 6);
```

## 二、新的对象和方法
### map
Map对象 用于保存键值对，任何JavaScript支持的值都可以作为一个键或者一个值。这听起来和对象差不多啊？其实它们还是有区别的：
- object的键只能是字符串或ES6的symbol值，而Map可以是任何值。
- Map对象有一个size属性，存储了键值对的个数，而object对象没有类似属性
```js
let myMap = new Map([['name','ren'],['age',12]]);
console.log(myMap);//{'name'=>'ren','age'=>12}
```
Map构造函数接受一个二维数组来创建一个Map对象，数组元素的第0位表示Map对象的key，第1位表示Map对象的value。
```js
myMap.set('sex','male')
console.log(myMap);//{'name'=>'ren','age'=>12,'sex'=>'male'}
myMap.get('name');//'ren'
```
Map对象使用set方法来新增数据，set方法接受两个参数，第一个表示key，第二个表示value; 使用get方法获取数据，参数是对象的key。
```js
myMap.has('age');//true
myMap.delete('age');//true
myMap.has('age');//false
myMap.get('age');//undefined
```
Map对象使用delete方法来删除数据，接收一个参数，表示需要被删除的key。
Map对象使用has方法检测是否已经具有某个属性，返回boolean值。

### Set
Set对象和Map对象类似，但它是用来存储一组唯一值的，而不是键值对。类似数组，但它的每个元素都是唯一的。
```js
let mySet = new Set([1,2,3]);
console.log(mySet);//{1,2,3}
mySet.add(4);
console.log(mySet);//{1,2,3,4}
mySet.delete(1);//true
mySet.has(1);//false
```
利用Set对象唯一性的特点，可以轻松实现数组的去重：
```js
let arr = [1,1,2,3,4,4];
let mySet = new Set(arr)
let newArr = Array.form(mySet)
console.log(newArr);//[1,2,3,4]

问题：哪些数据类型能去重
```

### 对象新特性
#### 创建对象的字面量方式可以更加简洁
直接使用变量名作为属性，函数体作为方法，最终变量值变成属性值，函数名变成方法名。
```js
let name = 'ren'
let age = 12
let myself = {
	name,
	age,
	say() {
		console.log(this.name)
	}
}
console.log(myself);//{name:'ren',age:12,say:fn}
myself.say();//'ren'
```

#### 对象的拓展运算符(...)三点
用于拷贝目标对象所有可遍历的属性到当前对象。

```js
let obj = {name:'ren',age:12};
let person = {...obj};
console.log(person);//{name:'ren',age:12}
obj == person;//false
let another = {sex:'male'};
let someone = {...person,...another};//合并对象
console.log(someone);//{name:'ren',age:12,sex:'male'}
```

#### ES6对象新增了两个方法，assign和is
assign用于浅拷贝源对象*可枚举属性*到目标对象。
```js
let source = {a:{ b: 1},b: 2};
let target = {c: 3};
Object.assign(target, source);
console.log(target);//{c: 3, a: {b:1}, b: 2}
source.a.b = 2;
console.log(target.a.b);//2
```

 什么情况下会使用assign

如果有同名属性，那么目标对象的属性值会被源对象的属性值覆盖。所以数组的表现就有一点特别了：
```js
Object.assign([1,2,3],[11,22,33,44]);//[11,22,33,44]
```
数组的index就是属性名，当使用assign方法时，从第0位开始，目标数组的值便开始被源数组的值覆盖了。

```js
let arr1 = [1,2]
let arr2 = [11,22]
Object.assign(arr1, arr2)
console.log(arr1) // [11, 22]
console.log(arr1 === arr2) // false
```

is方法和=== 功能基本类似，用于判断两个值是否绝对相等。
```js
1 Object.is(1,1);//true
2 Object.is(1,true);//false
3 Object.is([],[]);//false
4 Object.is(+0,-0);//false
5 Object.is(NaN,NaN);//true
```
他们仅有的两点区别是，is方法可以区分+0还是-0，还有就是它认为NaN是相等的。

### 字符串新方法
#### includes
includes()判断字符串是否包含参数字符串，返回boolean值。如果想要知道参数字符串出现的位置，还是需要indexOf或lastIndexOf方法。
#### startsWith()/endsWith()
判断字符串是否以参数字符串开头或结尾。返回boolean值。这两个方法可以有第二个参数，一个数字，表示开始查找的位置。
```js
1 let str = 'blue,red,orange,white';
2 str.includes('blue');//true
3 str.startsWith('blue');//true
4 str.endsWith('blue');//false
```
####  repeat()
repeat()方法按指定次数返回一个新的字符串。如果次数是大于0的小数则向下取整，0到-1之间的小数则向上取整，其他负数将抛出错误。
```js
console.log('hello'.repeat(2));//'hellohello'
console.log('hello'.repeat(1.9));//'hello'
console.log('hello'.repeat(-0.9));//''
console.log('hello'.repeat(-1.9));//error
```

#### padStart()/padEnd()
padStart()/padEnd()，用参数字符串按给定长度从前面或后面补全字符串，返回新字符串。
```js
1 let arr = 'hell';
2 console.log(arr.padEnd(5,'o'));//'hello'
3 console.log(arr.padEnd(6,'o'));//'helloo'
4 console.log(arr.padEnd(6));//'hell  ',如果没有指定将用空格代替
5 console.log(arr.padStart(5,'o'));//'ohell
```
另外，如果字符串加上补全的字符串超出了给定的长度，那么，超出的部分将被截去。

### 数组的新方法
#### of
of()是ES6新增的用于创建数组的方法。of把传入的参数当做数组元素，形成新的数组。
```js
let arr = Array.of(1,'2',[3],{})
console.log(arr);//[1,'2',[3],{}]
```
#### from
#### find 和findIndex
查找数组中符合条件的元素值或索引，方法不会修改原数组。
接受一个回调函数作为参数，函数可以接受四个参数，分别是`当前遍历到的元素`，`当前遍历的索引`，`数组本身`以及`函数内this指向`。方法会把回调函数作用于每一个遍历到的元素，如果遍历到某一个元素可以使回调函数返回true，那么find方法会立即返回该元素，findIndex方法会返回该元素的索引。并终止继续遍历。
如果有多个符合条件的，也将只返回第一个。如果遍历完整个数组也无法是回调函数返回true，那么find方法将返回undefined，findIndex方法将返回-1。
```js
let arr = [1,2,3,4,5];
console.log(arr.find((ele) => {
    return ele === 1;
}));//1
console.log(arr.findIndex((ele) => {
    return ele > 4;
}));//4
```

#### fill()/copyWithin()
替换数组中部分元素，`会修改原数组`。
```js
let arr = [1,2,3,4,5];
console.log(arr.fill(0,0,3));//[0,0,0,4,5]
//参数1表示目标值，参数2，3表示替换的始末位置，左闭右开区间。       
console.log(arr.copyWithin(0,2,4));//[0,4,0,4,5]
//参数1表示修改的起始位置，参数2，3表示用来替换的数据的始末位置，左闭右开区间。
```
fill()用指定的值替换，copyWithin()使用数组中原有的某一部分值替换。
includes()用于检测数组是否包含某个值，可以指定开始位置。

#### includes
includes()用于检测数组是否包含某个值，可以指定开始位置。
```js
let arr = [1,2,3,4,5];
console.log(arr.includes(2)); //true
console.log(arr.includes(1,1)); //false
```

### 三、函数
#### 参数默认值
ES6首次添加了参数默认值。我们再也不用在函数内部编写容错代码了。
```js
function add(a=1,b=2){
    return a + b;
}
add();//3
add(2);//4
add(3,4);//7
```
和参数默认值一起，ES6还带来了不定参。它的功能和使用arguments差不多。
```js
function add(...num){
    return num.reduce(function(result,value){
        return result + value;
    });
}
add(1,2,3,4);//10
```
下面介绍的箭头函数没有arguments属性，如果箭头函数内要实现不定参，上述方式就是一个不错的选择了。

#### 箭头函数
箭头函数实现了一种更加简洁的书写方式，并且也解决了关键字声明方式的一些麻烦事儿。箭头函数内部没有arguments，也没有prototype属性，所以不能用new关键字调用箭头函数。
箭头函数的书写方式：参数 => 函数体。
```js
let add = (a,b) => {
    return a+b;
}
let print = () => {
    console.log('hi');
}
let fn = a => a * a;
//当只有一个参数时，括号可以省略，函数体只有单行return语句时，大括号也可以省略，强烈建议不要省略它们，是真的难以阅读
```
当函数需要直接返回对象时，你必须使用小括号把对象包裹起来。否则将抛出错误。
```js
const fn = () => {name: 'ren', age:'12'}
// SyntaxError
const fn = () =>({name:'ren',age:12})
```

**箭头函数和普通函数最大的区别在于其内部this永远指向其父级AO对象的this。**

普通函数在预编译环节会在AO对象上添加this属性，保存一个对象（请参照[《JavaScript之深入对象（二）》](https://www.cnblogs.com/ruhaoren/p/11534871.html)）。每个普通函数在执行时都有一个特定的this对象，而箭头函数执行时并不直接拥有this属性，如果你在箭头函数中使用this，将根据函数作用域链，直接引用父级AO对象上this绑定的对象。普通函数的AO对象只有在函数执行时才产生，换言之，普通函数的this是由函数执行时的环境决定。而箭头函数的特别之处在于，当函数被定义时，就引用了其父级AO对象的this，即箭头函数的this由定义时的环境决定。
根据箭头函数的特点，不难推测：如果定义对象的方法直接使用箭头函数，那么函数内的this将直接指向window。

```js
var age = 123;
 let obj = {
     age:456,
     say:() => {
         console.log(this.age);
     }
 };
obj.say();//123
//对象是没有执行期上下文的（AO对象），定义对象的方法实际上是在全局作用域下，即window
```

如果你一定要在箭头函数中让this指向当前对象，其实也还是有办法的（但是没必要这么麻烦啊，直接使用普通函数不是更好吗？）：
```js
var age = 123;
 let obj = {
     age:456,
     say:function(){
         var fn = () => {
         console.log(this.age);
        }
         return fn();
     }
 };
obj.say();//456
```

我们来分析一下这是怎么做到的：首先，我们使用obj调用say方法时,say内创建了AO对象，并且该AO对象的this属性指向了obj（这里不明白的请回去复习一下我的[《JavaScript之深入函数/对象》](https://www.cnblogs.com/ruhaoren/p/11459963.html)），然后，say内部又声明了一个箭头函数。我们说箭头函数在声明时就要强行引用父级AO的this属性，那么现在该箭头函数的父级AO是谁呢？当然就是say的AO啦，所以这里箭头函数的this直接就绑定了obj，最后箭头函数在执行时拿到的this，实际上就是say方法的AO.this，即obj本身。
上面是在对象中使用箭头函数，如果那让你难于理解，那么请看下面这种方式：在普通函数中使用箭头函数。
```js
var obj = {name:'ren'};
function test(){
    var fn = () => {
        console.log(this);
    };
    fn();
}
test();//window
test.call(obj);//{name:'ren'}
```
test函数在全局执行时，其this指向window，这时也产生了箭头函数的定义，于是箭头函数内的this也被指向了window，所以最终打印出window对象。
当我们手动改变test函数执行时this的指向时，箭头函数定义所绑定的this实际上也被我们修改了，所以最终打印出obj

### class类
class作为对象的模板被引入es6,你可以通过class关键字定义类，class的本质依然是一个函数
#### 创建类
```js
class Ex {
	constructor(name){
		this.name = name
		this.say = () => {
			console.log(this.name)
		}
	}
	methods() {
		console.log('hello'+this.name)
	}
	static a = 123
	static m = () => {
		console.log(this.a)
	}
}
// let ex = class {} // 字面量方式
var example = new Ex('ren')
example.say() // ren
Ex.m() // 123
example.methods();//'hello ren'
```
constructor是创建类必须的方法，当使用new调用类创建实例时，将自动执行该方法，该方法和构造函数类似，默认返回this对象。**实例的方法和属性都定义在constructor内部**。相当于构造函数的this方式。
类保留了prototype属性，类中的方法不需要使用function关键字，并且方法之间不需要逗号隔开。**类中定义的方法实际上还是保存在类的prototype属性上**。
使用static关键字定义类的静态属性和方法。类中不能定义共有属性，要想定义实例的共有属性还是需要使用prototype属性：Ex.prototype.属性名 = 属性值。

创建实例依然使用new关键字。

#### 类的继承  
类的继承通过extends关键字实现。
```js
class Person {
	constructor (name,age){
		this.name = name
		this.age = age
	}
	say() {
		console.log(this.name + ':' + this.age)
	}
}

class Student extends Person {
	constructor (name,age,sex){
		super(name, age)
		this.sex = sex
	}
}
var student = new Student('ren',12,'male');
student.name;//'ren'
student.sex;//'male'
student.say();//'ren:12'
```
子类继承自父类，不会隐式的创建自己的this对象，而是通过super()引用父类的this。这个过程和在子构造函数内使用父构造函数call(this)很像，但他们有本质的区别。另外，ES6规定，super()必须在子类的this之前执行。所以一般我们把super()放在子类constructor方法的第一行，这样准没错！

### 模块的导入和导出
#### 导入
ES6使用关键字 import 导入模块（文件），有两种常用的方式：
```js
import ‘模块名称’ from ‘路径’；
import  ‘路径’；
```
通过 import...from 的方式引入模块，模块名称实际上相当于定义一个变量，用来接收即将导入的模块。路径可以有很多方式，既可以是绝对路径，也可以是相对路径，甚至只是一个简单的模块名称，更甚至连文件后缀都不需要。当你使用该命令时，系统会自动从配置文件中指定的路径中去寻找并加载正确的文件。
```js
import Vue from "vue";
//完整路劲其实是 "../node_modules/vue/dist/vue.js";
```
通过 import... 的方式一般用来引入样式文件或预处理文件，因为他们不需要用变量来接收。

#### 导出
ES6 通过 export 和export default 导出模块。导出的含义是向外暴露、输出，在一个文件中通过 import 导入另一个文件，通过变量即可以接收到导出的数据了。一般情况下，JS文件可以向外输出变量、函数和对象。
```js
let name = 'ren',age = 12;
export {name,age};
//注意：变量需要用大括号包裹，然后才能向外输出
```
如果仅需向外暴露一个变量：
```js
export var name = 'ren';
```
使用 export 向外输出函数的用法和变量相同

**总结：使用 export 向外输出成员时，可以同时输出多个，并且必须用‘｛｝’大括号包裹，在其他地方使用 import 导入时，接收成员的变量名必须和这里输出的名称一致，同时，可以根据实际情况，仅接收实际需要的的成员（接收的时候也要用大括号包裹）。**

如果希望通过 export 向外暴露成员，并且在导入的时候自定义接收名称，那么你可以使用 as 关键字重命名。
```js
let name = 'ren', age = 12
export {name, age}
import {name as myName, age as myAge} from 'url'
```
与 export 相比，export default 有以下几点不同：首先，在同一个模块中，export default 只允许向外暴露一次成员；然后，这种方式可以使用任意的名称接收，不像 export 那样有严格的要求；最后，export 和 export default 可以在同一模块中同时存在。
```js
let person = {name:'ren'};
let age = 12;
let address = 'cd';
export default person;
export {age};
export {address};
**************************
import man,{age,address} from 'url'
```

### 异步机制
ES6新增了两种实现异步的新机制，Promise和Generator
#### promise
##### 含义
Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了`Promise`对象。

所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

**特点**
（1）对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

**缺点**
首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。
其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。
第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

##### 基本用法
ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。
```js
cosnt promise = new Promise(function(resolve, reject){
	// some code
	if(异步操作成功){
		resolve(value)
	} else {
		reject(error)
	}
})
```
`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
......

注意，调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行。
```javascript
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```
上面代码中，调用`resolve(1)`以后，后面的`console.log(2)`还是会执行，并且会首先打印出来。这是因为立即 resolved 的 Promise 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

一般来说，调用`resolve`或`reject`以后，Promise 的使命就完成了，后继操作应该放到`then`方法里面，而不应该直接写在`resolve`或`reject`的后面。所以，最好在它们前面加上`return`语句，这样就不会有意外。

```javascript
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})
```
##### Promise.prototype.then()
Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象`Promise.prototype`上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，`then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的。

`then`方法返回的是一个新的`Promise`实例（注意，不是原来那个`Promise`实例）。因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

##### Promise.prototype.catch()
`Promise.prototype.catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数。

##### Promise.prototype.finally()
`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
```javascript
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```
`finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是`fulfilled`还是`rejected`。这表明，`finally`方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。
`finally`本质上是`then`方法的特例。

```javascript
promise
.finally(() => {
  // 语句
});

// 等同于
promise
.then(
  result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```

上面代码中，如果不使用`finally`方法，同样的语句需要为成功和失败两种情况各写一次。有了`finally`方法，则只需要写一次。


##### Promise.all
`Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
##### promise.race
`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```javascript
const p = Promise.race([p1, p2, p3]);
```
上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数。
```javascript
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
```

上面代码中，如果 5 秒之内`fetch`方法无法返回结果，变量`p`的状态就会变为`rejected`，从而触发`catch`方法指定的回调函数。

##### Promise.allSettled
有时候，我们希望等到一组异步操作都结束了，不管每一个操作是成功还是失败，再进行下一步操作。但是，现有的 Promise 方法很难实现这个要求。

`Promise.all()`方法只适合所有异步操作都成功的情况，如果有一个操作失败，就无法满足要求。

```javascript
const urls = [url_1, url_2, url_3];
const requests = urls.map(x => fetch(x));

try {
  await Promise.all(requests);
  console.log('所有请求都成功。');
} catch {
  console.log('至少一个请求失败，其他请求可能还没结束。');
}
```
`Promise.allSettled()`方法接受一个数组作为参数，数组的每个成员都是一个 Promise 对象，并返回一个新的 Promise 对象。只有等到参数数组的所有 Promise 对象都发生状态变更（不管是`fulfilled`还是`rejected`），返回的 Promise 对象才会发生状态变更。