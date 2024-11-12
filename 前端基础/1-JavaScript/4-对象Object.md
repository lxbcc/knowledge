[javascript中object方法有哪些-js教程-PHP中文网](https://www.php.cn/faq/473821.html)
[js Object方法大全\_枫行-的博客-CSDN博客](https://blog.csdn.net/duguxueao/article/details/123771968)
[JS对象——defineProperty方法简介\_前水员的博客-CSDN博客](https://blog.csdn.net/qq_43465434/article/details/114454472)
## 基础
### 对象的分类：
1. 内建对象
   由ES标准中定义的对象，在任何的ES的实现中都可以使用
   比如：Math String Number Boolean Function Object...

2. 宿主对象
   由JS的运行环境提供的对象，目前来讲主要是指浏览器提供的对象
   比如：BOM DOM

3. 自定义对象
   由开发人员自己创建的对象

### 对象定义
- 无序的数据集合
- 键值对的集合 ( key: value ) object 是七种数据类型中唯一一种复杂类型。
### 创建对象

1. 构造函数
   使用<mark class="hltr-cyan">new关键字</mark>调用的函数，是构造函数constructor，构造函数是专门用来创建对象的函数
   let obj = new Object() 

2. 字面量
   let obj = {}

**★ 注意 ：**
- **键名'name'， 'age'加了引号说明是字符串，引号可省略，就算省略掉引号还是字符串**
- **键名是字符串，不是标识符，∵ 键名可以用数字开头**

### 对象的调用

- 对象里面的属性调用 : 对象.属性名 ，这个小点 . 就理解为“ 的 
- 对象里面属性的另一种调用方式 : 对象[‘属性名’]，注意方括号里面的属性必须加引号，我们后面会用
- 对象里面的方法调用：对象.方法名() ，注意这个方法名字后面一定加括号
## 对象的常用操作

### 查看所有属性

查看自身属性
```js
// 自身属性

let obj = {name: 'Mia', age: 18}

console.log(Object.keys(obj)) // 查看自身属性名
console.log(Object.values(obj)) // 查看自身属性值
console.log(Object.entries(obj)) // 查看自身属性名和值
```

查看 自身属性 和 共有属性
```js
// console.dir()可以显示一个对象所有的属性和方法
// 显示指定JavaScript对象的属性列表
console.dir(obj)
```

查看共有属性（不推荐）
```js
obj.__proto__    // 不推荐
```

判断一个属性 'xxx' 是自身的还是共有的
```js
obj.hasOwnProperty('xxx')     // true就是自身的，false就是共有或不存在
```

<mark class="hltr-yellow">'xxx' in obj 不能判断出这个属性是否是自身属性还是共有属性,可以验证是否在该对象中
obj.hasOwnProperty('xxx') 可以判断出这个属性是否是自身属性</mark>


拓展：
<mark class="hltr-cyan">console.table()</mark>方法用于在控制台中以表格形式显示数组或对象的数据。这对于可视化复杂数据结构非常有用，使数据更易于阅读和理解。
### 查看某个属性
[]语法
```js
obj['key']      // 'key'是字符串, 不加引号指的是变量
```

点语法
```js
obj.key       // key 也是字符串'key'
```

变量
```js
obj[变量名]

// []代表用a 这个变量的值作属性名，而不是字符串'a'作属性名
let a = 'xxx'
let obj = { [a]: 1111 }

```

例题
```js
let list = ['name', 'age', 'gender']
let person = {
       name:'Mia', age:18, gender:'female'}
for(let i = 0; i < list.length; i++){
  let name = list[i]
  console.log(person__???__)
}
// 需要将 person 的所有属性被打印出来，应选择下方哪个选项？
console.log(person.name)     // Mia Mia Mia
console.log(person[name])    // Mia 18 female  符合题意✓
```

### 修改对象的属性值

```js
obj.name="tom"；
obj["name"]="tom";
```

### 删除对象的属性
```js
obj.name = undefined
delete obj.name;
delete obj['name'];
```

### 检测对象的某个属性是否在对象里
```js
"属性名" in 对象名;
对象名.hasOwnProperty("属性名");

for (变量 in 对象名字) {
    // 在此执行代码
}
```

```js
/* 
对象的修改，删除，检测某个属性是否在对象里
一.修改
二.删除
三.in检测属性是否存在,相同的方法：hasOwnProperty();
 */
 var Student={
	 name:"jason",
	 age:20,
	 like:function(){
		 console.log("钓鱼");
	 }
 }
 //修改
 Student['age']=25;
 console.log(Student);//03.对象的属性修改，删除，in.html:23 {name: "jason", age: 25, like: f}
 //删除
 delete Student['like'];
 console.log(Student);//03.对象的属性修改，删除，in.html:26 {name: "jason", age: 25}
 //in检测,相同的方法：hasOwnProperty();
 console.log("name" in Student);//true
 console.log("color" in Student);//false
 console.log(Student.hasOwnProperty("name"));//true
```


### new关键字
new在执行时会做四件事情：
1. 在内存中创建一个新的空对象。
2. 让 this 指向这个新的对象。
3. 执行构造函数里面的代码，给这个新对象添加属性和方法。
4. 返回这个新对象（所以构造函数里面不需要return）。
```js
function One(name,age){
	this.name=name;
	this.age=age;
}
function myNew(fn,...args){
	let obj={};
	obj.__proto__=fn.prototype;
	let res=fn.apply(obj,args);
	if(res instanceof Object){
		return res;
	}else{
		return obj;
	}
}
let person=myNew(One,"张三",18);

```

## 对象方法
### create 创建一个对象

```js
const obj = Object.create({a:1}, {b: {value: 2}})
 
// 第一个参数为对象，对象为函数调用之后返回新对象的原型对象
// 第二个参数为对象本身的实例方法（默认不能修改,不能枚举）
obj.__proto__.a === 1      // true 
 
obj.b = 3;
console.log(obj.b)      // 2
 
//创建一个可写的,可枚举的,可配置的属性p
obj2 = Object.create({}, {
  p: {
    value: 2,       // 属性值
    writable: true,     //  是否可以重写值
    enumerable: true,   //是否可枚举
    configurable: true  //是否可以修改以上几项配置
  }
});
 
obj2.p = 3;
console.log(obj2.p)     // 3
 
注意： enumerable 会影响以下

for…in  遍历包括对象原型上属性
Object.keys()   只能遍历自身属性
JSON.stringify  只能序列化自身属性
```

### defineProperty

Object.defineProperty(object, prop, descriptor)定义对象属性
1. <mark class="hltr-cyan">参数一 obj</mark>
   绑定属性的目标对象
2. <mark class="hltr-cyan">参数二 propety</mark>
   绑定的属性名
3. <mark class="hltr-cyan">参数三：descriptor</mark>
   属性描述（配置），且此参数本身为一个对象
   - 属性值1：<mark class="hltr-green">value</mark> 设置属性默认值
   - 属性值2：<mark class="hltr-green">writable</mark>  设置属性是否能够修改
   - 属性值3：<mark class="hltr-green">enumerable</mark>  设置属性是否可以枚举，即是否允许遍历
   - 属性值4：<mark class="hltr-green">configurable</mark>  设置属性是否可以删除
   - 属性值5：<mark class="hltr-green">get</mark>  获取属性的值
   - 属性值6：<mark class="hltr-green">set</mark>  设置属性的值

创建一个对象obj
```js
var obj = {}
```

通过value设置属性值
```js
Object.defineProperty(obj,'count',{
	value: 5
})
console.log(obj);
```

通过writable设置是否可写/修改
```js
Object.defineProperty(obj, "newDataProperty", {
    writable:false
})
```

```js
添加数据属性
var obj = {};
 
// 1.添加一个数据属性
Object.defineProperty(obj, "newDataProperty", {
    value: 101,
    writable: true,
    enumerable: true,
    configurable: true
});
 
obj.newDataProperty    // 101
 
// 2.修改数据属性
Object.defineProperty(obj, "newDataProperty", {
    writable:false
});
 
//添加访问器属性
var obj = {};
 
Object.defineProperty(obj, "newAccessorProperty", {
    set: function (x) {
        this.otherProperty = x;
    },
    get: function () {
        return this.otherProperty;
    },
    enumerable: true,
    configurable: true
});

// 报错
Object.defineProperty(obj, 'c', {
	value: 3,
	get: function (value) {
		return value
	}
})

console.log(obj)
// Invalid property descriptor. Cannot both specify accessors and a value or writable attribute
 
注意：
1.第一个参数必须为对象
2.descriptor 不能同时具有 （value 或 writable 特性）（get 或 set 特性）。
3.configurable 为false 时，不能重新修改装饰器
```

### 其他方法

- Object.defineProperties()
  给对象添加多个属性并分别指定它们的配置。
```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```
  
- Object.keys()
  返回一个包含所有给定对象自身可枚举属性名称的数组。
  
- Object.values()
  返回给定对象自身可枚举值的数组。
  
- Object.entries()
  返回给定对象自身可枚举属性的 [key, value] 数组。
```js
  [[ 'a', 1 ], [ 'b', 2 ]]
```

- hasOwnProperty() 
  方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性
  
- Object.is()
  比较两个值是否相同。所有 NaN 值都相等（这与== 和=== 不同）。
  
- Object.assign()
  通过复制一个或多个对象来创建一个新的对象。
```js
const target = { a: 1, b: 1 };
const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2); target // {a:1, b:2, c:3}

特殊情况：
let obj = {a: 1};
Object.assign(obj, undefined) === obj  // true
Object.assign(obj, null) === obj       // true
Object.assign([1, 2, 3], [4, 5])        // [4, 5, 3]


Object.assign方法实行的是浅拷贝，而不是深拷贝。
const obj1 = {a: {b: 1}};
const obj2 = Object.assign({}, obj1);
 
obj1.a.b = 2;
console.log(obj2.a.b) //2
obj2.a.b = 3
console.log(obj1.a.b) //3
 
```
  
- valueOf 需要返回对象的原始值

```js
备注：js对象中的valueOf()方法和toString()方法非常类似，但是，当需要返回对象的原始值而非字符串的时候才调用它，尤其是转换为数字的时候。如果在需要使用原始值的上下文中使用了对象，JavaScript就会自动调用valueOf()方法。
 
const o = {a: 1, valueOf: function(){ return 123123 } }
Number(o)    //123123
 
// 给大家出一个题
const o2 = {
    x: 1,
    valueOf: function(){
        return this.x++
    }
}
 
if(o2 == 1 && o2 == 2 && o2 == 3){
    console.log('down')
    console.log(o2.x) // 4
}else{
    console.log('faild')
}
 
// Array：返回数组对象本身
var array = ["CodePlayer", true, 12, -5];
array.valueOf() === array;   // true
 
// Date：当前时间距1970年1月1日午夜的毫秒数
var date = new Date(2013, 7, 18, 23, 11, 59, 230);
date.valueOf()     // 1376838719230
 
// Number：返回数字值
var num =  15.26540;
num.valueOf()     // 15.2654
 
// 布尔：返回布尔值true或false
var bool = true;
bool.valueOf() === bool    // true
// new一个Boolean对象
var newBool = new Boolean(true);
// valueOf()返回的是true，两者的值相等
newBool.valueOf() == newBool     // true
// 但是不全等，两者类型不相等，前者是boolean类型，后者是object类型
newBool.valueOf() === newBool     // false
 
// Function：返回函数本身
function foo(){ 
}
foo.valueOf() === foo      // true
var foo2 =  new Function("x", "y", "return x + y;");
foo2.valueOf() === foo2    // true
 
// Object：返回对象本身
var obj = {name: "张三", age: 18};
obj.valueOf() === obj        // true
 
// String：返回字符串值
var str = "http://www.365mini.com";
str.valueOf() === str       // true
// new一个字符串对象
var str2 = new String("http://www.365mini.com");
// 两者的值相等，但不全等，因为类型不同，前者为string类型，后者为object类型
str2.valueOf() === str2      // false
 
```

#### 浏览器对象
[Browser 对象所有属性和方法介绍，看这一篇就够了！\_browser方法\_CODER-V的博客-CSDN博客](https://blog.csdn.net/qq_45872039/article/details/129153609)