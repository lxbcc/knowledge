## 1. 定义
执行上下文就是<mark style="background: #FFB86CA6;">JavaScript</mark>代码被解析和执行时所在环境的抽象概念

## 2. 执行上下文的类型
**全局执行上下文**
代码首次执行时候的默认环境，任何不在函数内部的代码都在全局上下文中
一个程序中只能存在一个全局执行上下文
它会执行两件事：
* 创建一个全局对象，在浏览器中这个全局对象就是 window 对象
* 设置 this 的值等于这个全局对象
**函数执行上下文**
每次调用函数（包括多次调用同一个函数）时，都会为该函数创建一个新的执行上下文
每个函数都拥有自己的执行上下文，但是只有在<mark style="background: #FFF3A3A6;">函数被调用时才会被创建</mark>。
一个程序中可以存在任意数量的函数执行上下文。
**eval函数执行上下文**
js的eval函数执行其内部的代码会创建属于自己的执行上下文, 很少用而且不建议使用。

## 3. 执行上下文的特点
1.  单线程，只在主线程上运行；
2.  同步执行，从上向下按顺序执行；
3.  全局上下文只有一个，也就是window对象；
4.  函数执行上下文没有限制；(无数的函数上下文)
5.  函数每调用一次就会产生一个新的执行上下文环境, 即使是调用自身


## 4. 执行栈

**定义**：管理多个执行上下文靠的就是**执行栈**，也被叫做**调用栈**。
**特点**：后进先出（LIFO）的结构。
**作用**：存储在代码执行期间的所有执行上下文。

`js`在首次执行的时候，会创建一个**全局执行上下文**并推入栈中。
每当有函数被调用时，引擎都会为该函数创建一个新的**函数执行上下文**然后推入栈中。
当栈顶的函数执行完毕之后，该函数对应的**执行上下文**就会从执行栈中`pop`出，然后上下文控制权移到下一个执行上下文。
```js
var a = 1; // 1. 全局上下文环境
function bar (x) {
	console.log('bar') 
	var b = 2; fn(x + b); // 3. fn上下文环境
} function 
fn (c) { console.log(c); } 
bar(3); // 2. bar上下文环境
```
![[Pasted image 20230730222456.png]]



## 5. 执行上下文的生命周期

执行上下文的生命周期也非常容易理解, 分为三个阶段:

1.  创建阶段
2.  执行阶段(入栈执行)
3.  销毁阶段(出栈回收)

### 创建阶段

js执行上下文的创建阶段主要负责三件事：

1.  **this** 值的决定，即我们所熟知的 **this 绑定**。
2.  创建**词法环境**组件（LexicalEnvironment）。
3.  创建**变量环境**组件（VariableEnvironment）。
```js
ExecutionContext = {  
    // 确定this的值
    ThisBinding = <this value>,
    // 创建词法环境组件
    LexicalEnvironment = {},
    // 创建变量环境组件
    VariableEnvironment = {},
}
```
![[Pasted image 20230807173458.png]]
#### this 绑定
-   全局执行上下文中, `this`指的就是全局对象, 浏览器环境指向`window`对象, `nodejs`中指向这个文件的`module`对象.
- 在函数执行上下文中，`this`的值取决于该函数是如何被调用的。如果它被一个引用对象调用，那么`this`会被设置成那个对象，否则`this`的值被设置为全局对象或者`undefined`（在严格模式下）。
[js 五种绑定彻底弄懂this，默认绑定、隐式绑定、显式绑定、new绑定、箭头函数绑定详解 - 听风是风 - 博客园](https://www.cnblogs.com/echolun/p/11962610.html)

#### 词法环境
如上图，由两个部分组成
1. **环境记录**: 存储变量和函数声明的实际位置;
2. **对外部环境的引用**: 用于访问其外部词法环境. （
3. **外部环境的引用**意味着它可以访问其父级词法环境（作用域））
同样的, 词法环境也主要有两种类型:

- **全局环境**: 拥有一个全局对象(window对象)及其关联的所有属性和方法(比如数组的方法splice、concat等), 同时也包含了用户自定义的全局变量. 但是<mark style="background: #FFF3A3A6;">全局环境中没有外部环境的引用</mark>, 也就是外部环境引用为<mark style="background: #FFF3A3A6;">null</mark>.

- **函数环境**: 用户在函数中自定义的变量和函数存储在**环境记录**中, 包含了`arguments`对象. 而对外部环境的引用可以是<mark style="background: #FFF3A3A6;">全局环境</mark>， 也可以是另一个**函数环境**(比如一个函数中包含了另一个函数).

**环境记录器**也有两种类型：

1. 在**全局环境**中，环境记录器是**对象环境记录器**，用来定义出现在**全局上下文**中的变量和函数的关系。
2. 在**函数环境**中，环境记录器是**声明式环境记录器**，用来存储变量、函数和参数。
伪代码
```js
// 全局环境
GlobalExectionContext = {
    // 全局词法环境
    LexicalEnvironment: {
        // 环境记录
        EnvironmentRecord: {
            Type: "Object", //类型为对象环境记录
            // 标识符绑定在这里 
        },
        outer: < null >
    }
};
// 函数环境
FunctionExectionContext = {
    // 函数词法环境
    LexicalEnvironment: {
        // 环境纪录
        EnvironmentRecord: {
            Type: "Declarative", //类型为声明性环境记录
            // 标识符绑定在这里 
        },
        outer: < Global or outerfunction environment reference >
    }
};
```

#### 变量环境
**变量环境**其实也是一个词法环境, 因此它具有上面定义的词法环境的所有属性

在 ES6 中，**词法环境**和 **变量环境**的区别在于前者用于存储**函数声明和变量（ `let` 和 `const` ）绑定，而后者仅用于存储变量（ var ）绑定**。

```js
let a = 20;  
const b = 30;  
var c;

function multiply(e, f) {  
 var g = 20;  
 return e * f * g;  
}

c = multiply(20, 30);

// 我们用伪代码来描述上述代码中执行上下文的创建过程：

//全局执行上下文
GlobalExectionContext = {
    // this绑定为全局对象
    ThisBinding: <Global Object>,
    // 词法环境
    LexicalEnvironment: {  
        //环境记录
      EnvironmentRecord: {  
        Type: "Object",  // 对象环境记录
        // 标识符绑定在这里 let const创建的变量a b在这
        a: < uninitialized >,  
        b: < uninitialized >,  
        multiply: < func >  
      }
      // 全局环境外部环境引入为null
      outer: <null>  
    },
    // 变量环境
    VariableEnvironment: {  
      EnvironmentRecord: {  
        Type: "Object",  // 对象环境记录
        // 标识符绑定在这里  var创建的c在这
        c: undefined,  
      }
      // 全局环境外部环境引入为null
      outer: <null>  
    }  
  }

  // 函数执行上下文
  FunctionExectionContext = {
     //由于函数是默认调用 this绑定同样是全局对象
    ThisBinding: <Global Object>,
    // 词法环境
    LexicalEnvironment: {  
      EnvironmentRecord: {  
        Type: "Declarative",  // 声明性环境记录
        // 标识符绑定在这里  arguments对象在这
        Arguments: {0: 20, 1: 30, length: 2},  
      },  
      // 外部环境引入记录为</Global>
      outer: <GlobalEnvironment>  
    },
    // 变量环境
    VariableEnvironment: {  
      EnvironmentRecord: {  
        Type: "Declarative",  // 声明性环境记录
        // 标识符绑定在这里  var创建的g在这
        g: undefined  
      },  
      // 外部环境引入记录为</Global>
      outer: <GlobalEnvironment>  
    }  
  }
```

## 关于变量对象与活动对象

变量对象与活动对象的概念是 ES3 提出的老概念，从 ES5 开始就用词法环境和变量环境替代

在上文中，我们通过介绍词法环境与变量环境解释了为什么var会存在变量提升，为什么let const没有，而通过变量对象与活动对象是很难解释的，由其是在JavaScript在更新中不断在弥补当初设计的缺陷。

其次，词法环境的概念与变量对象这类概念也是可以对应上的。

我们知道变量对象与活动对象其实都是变量对象，变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。而在函数上下文中，我们用活动对象(activation object, AO)来表示变量对象。

那这不正好对应到了全局词法记录与函数词法记录了吗。而且由于ES6新增的let const不存在变量提升，于是正好有了词法环境与变量环境的概念来解释这个问题。

所以说到这，你也不用为词法环境，变量对象的概念闹冲突了。

## var变量提升与let和const
变量提升是指在js代码执行过程中，js引擎把变量的声明部分和函数声明部分提升到代码开头的“行为”。变量被提升后，会给变量设置默认值undefined

**变量提升的实现并不是物理地移动代码的位置，而是在编译阶段被js引擎放入内存中。**

普通变量提升会赋值为undefined，函数变量名会将整个函数提升
```js
console.log(fn);  // [Function: fn]
console.log(a);  // undefined

function fn() {
    console.log(111);
}
var a = 1

```

- 函数表达式只会将变量提升，不会将函数题提升
    
- 当有多个相同类型的声明（同样是函数声明或者同样是普通变量声明），最后一个声明会覆盖之前的声明
    
- 当函数声明与变量声明同时出现时，函数声明优先级更高

**暂时性死区**:Temporal Dead Zone 简称 TDZ
在ES6中, 引入了 let 和 const 两个新的命令, 并且使用这两个命令定义的变量不存在变量提升, 且使用let和const声明变量之前, 该变量都是不可用的, 这在语法上被称为 暂时性死区


参考
[js执行上下文 - 简书](https://www.jianshu.com/p/42c2ffab400c)
[JavaScript深入之执行上下文栈和变量对象 | 木易杨前端进阶](https://muyiy.cn/blog/1/1.2.html#%E5%8F%82%E8%80%83)
[面试官：那我们来说说执行上下文吧 - 知乎](https://zhuanlan.zhihu.com/p/141582658)
[从面试题的角度深入理解执行栈和执行上下文 - 知乎](https://zhuanlan.zhihu.com/p/152207264)