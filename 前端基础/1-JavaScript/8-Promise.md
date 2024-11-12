### 异步编程的实现方式
- <mark class="hltr-cyan">回调函数</mark> 的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。
- <mark class="hltr-cyan">Promise</mark> 的方式，使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成多个 then 的链式调用，可能会造成代码的语义不够明确。
- <mark class="hltr-cyan">generator</mark> 的方式，它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。当遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕时再将执行权给转移回来。因此在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式需要考虑的问题是何时将函数的控制权转移回来，因此需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。
- <mark class="hltr-cyan">async函数</mark> 的方式，async 函数是 generator 和 promise 实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个 await 语句的时候，如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。

### setTimeout、Promise、Async/Await 的区别
#### （1）setTimeout
```js
console.log('script start')	//1. 打印 script start
setTimeout(function(){
    console.log('settimeout')	// 4. 打印 settimeout
})	// 2. 调用 setTimeout 函数，并定义其完成后执行的回调函数
console.log('script end')	//3. 打印 script start
// 输出顺序：script start->script end->settimeout
```

#### （2）Promise

Promise本身是**同步的立即执行函数**， 当在executor中执行resolve或者reject的时候, 此时是异步操作， 会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法执行，打印p的时候，是打印的返回结果，一个Promise实例。
```js
console.log('script start')
let promise1 = new Promise(function (resolve) {
    console.log('promise1')
    resolve()
    console.log('promise1 end')
}).then(function () {
    console.log('promise2')
})
setTimeout(function(){
    console.log('settimeout')
})
console.log('script end')
// 输出顺序: script start->promise1->promise1 end->script end->promise2->settimeout
```

当JS主线程执行到Promise对象时：

- promise1.then() 的回调就是一个 task
- promise1 是 resolved或rejected: 那这个 task 就会放入当前事件循环回合的 microtask queue
- promise1 是 pending: 这个 task 就会放入 事件循环的未来的某个(可能下一个)回合的 microtask queue 中
- setTimeout 的回调也是个 task ，它会被放入 macrotask queue 即使是 0ms 的情况
#### async/await
```js
async function async1(){
   console.log('async1 start');
    await async2();
    console.log('async1 end')
}
async function async2(){
    console.log('async2')
}
console.log('script start');
async1();
console.log('script end')
// 输出顺序：script start->async1 start->async2->script end->async1 end

```
async 函数返回一个 Promise 对象，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了 async 函数体。


### Promise理解
（1）Promise的实例有**三个状态**:
- Pending（进行中）
- Resolved（已完成）
- Rejected（已拒绝）

当把一件事情交给promise时，它的状态就是Pending，任务完成了状态就变成了Resolved、没有完成失败了就变成了Rejected。

（2）Promise的实例有**两个过程**：
- pending -> fulfilled : Resolved（已完成）
- pending -> rejected：Rejected（已拒绝）
注意：一旦从进行状态变成为其他状态就永远不能更改状态了。

**Promise的特点：**

- 对象的状态不受外界影响。promise对象代表一个异步操作，有三种状态，`pending`（进行中）、`fulfilled`（已成功）、`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态，这也是promise这个名字的由来——“**承诺**”；
- 一旦状态改变就不会再变，任何时候都可以得到这个结果。promise对象的状态改变，只有两种可能：从`pending`变为`fulfilled`，从`pending`变为`rejected`。这时就称为`resolved`（已定型）。如果改变已经发生了，你再对promise对象添加回调函数，也会立即得到这个结果。这与事件（event）完全不同，事件的特点是：如果你错过了它，再去监听是得不到结果的。

**Promise的缺点：**

- 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
- 当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
### Promise的基本用法

#### （1）创建Promise对象
Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`
```js
const promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});

```
**一般情况下都会使用**`new Promise()`**来创建promise对象，但是也可以使用**`promise.resolve`**和**`promise.reject`**这两个方法：**

##### Promise.resolve
类方法，该方法返回一个以 value 值解析后的 Promise 对象 
- 如果这个值是个 thenable（即带有 then 方法）(ajax)，返回的 Promise 对象会“跟随”这个 thenable 的对象，采用它的最终状态（指 resolved/rejected/pending/settled） 
- 如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。  
- 其他情况以该值为成功状态返回一个 Promise 对象。
```js
//如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。  
function fn(resolve){
    setTimeout(function(){
        resolve(123);
    },3000);
}
let p0 = new Promise(fn);
let p1 = Promise.resolve(p0);
// 返回为true，返回的 Promise 即是 入参的 Promise 对象。
console.log(p0 === p1);

```
`Promise.resolve(value)`的返回值也是一个promise对象，可以对返回值进行.then调用，代码如下：
```js
Promise.resolve(11).then(function(value){
  console.log(value); // 打印出11
});

```
`resolve(11)`代码中，会让promise对象进入确定(`resolve`状态)，并将参数`11`传递给后面的`then`所指定的`onFulfilled` 函数；

##### Promise.reject
`Promise.reject` 也是`new Promise`的快捷形式，也创建一个promise对象
```js
console.log(Promise.reject(11)) 
// Promise {<rejected>: 12}

let p = new Promise((resolve, reject)=> {})
console.log(Promise.reject(p))
// Promise { <rejected> Promise { <pending> } }
```


```js
function testPromise(ready) {
  return new Promise(function(resolve,reject){
    if(ready) {
      resolve("hello world");
    }else {
      reject("No thanks");
    }
  });
};
// 方法调用
testPromise(true).then(function(msg){
  console.log(msg);
},function(error){
  console.log(error);
});

```
上面的代码的含义是给`testPromise`方法传递一个参数，返回一个promise对象，如果为`true`的话，那么调用promise对象中的`resolve()`方法，并且把其中的参数传递给后面的`then`第一个函数内，因此打印出 “`hello world`”, 如果为`false`的话，会调用promise对象中的`reject()`方法，则会进入`then`的第二个函数内，会打印`No thanks`；
  
#### （2）Promise方法
Promise有五个常用的方法：then()、catch()、all()、race()、finally。下面就来看一下这些方法
##### .then
```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});

```
`then`方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为`resolved`时调用，第二个回调函数是Promise对象的状态变为`rejected`时调用。其中第二个参数可以省略。 `then`方法返回的是一个新的Promise实例（不是原来那个Promise实例）。因此可以采用链式写法，即`then`方法后面再调用另一个then方法。

```js
let promise = new Promise((resolve,reject)=>{
    ajax('first').success(function(res){
        resolve(res);
    })
})
promise.then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    
})
```
promise.then返回的新的promise的结果状态由什么决定
1. 通过return 返回一个非promise的值，则新promise的状态fulfilled，值为return 的值
2. 不做任何处理（不return == return undefined），所以根据结论1新promise的状态为fulfilled，值为undefined
3. 通过throw主动抛出错误或者代码出现错误，则promise的状态为rejected，值为throw的值
4、通过return 返回一个promise对象，则新promise就是return的promsie

##### .catch
Promise对象除了有then方法，还有一个catch方法，该方法相当于`then`方法的第二个参数，指向`reject`的回调函数。不过`catch`方法还有一个作用，就是在执行`resolve`回调函数时，如果出现错误，抛出异常，不会停止运行，而是进入`catch`方法中。
```js
p.then((data) => {
     console.log('resolved',data);
},(err) => {
     console.log('rejected',err);
     }
); 
p.then((data) => {
    console.log('resolved',data);
}).catch((err) => {
    console.log('rejected',err);
});

```

##### . all()
[Promise.all（） - 是桂 - 博客园](https://www.cnblogs.com/rickdiculous/p/13580329.html)
[Promise.all 使用方法\_51CTO博客\_promise.all](https://blog.51cto.com/u_15105778/6083783)
[Promise.all和promise.race的应用场景举例 - 知乎](https://zhuanlan.zhihu.com/p/402219884)
[Promise.all() 原理解析及使用指南 - 掘金](https://juejin.cn/post/7003713678419705870?searchId=20231010195816955BC21C05B262E95CCB)

`all`方法可以完成并行任务， 它接收一个数组，数组的每一项都是一个`promise`对象。当数组中所有的`promise`的状态都达到`resolved`的时候，`all`方法的状态就会变成`resolved`，如果有一个状态变成了`rejected`，那么`all`方法的状态就会变成`rejected`。
成功的时候返回的是一个结果数组，而失败的时候则返回最先被reject失败状态的值

await 是可以获得多个promise 返回结果的，Promise.all()返回的也是promise结果。所以想要使用await 拿到多个promise的返回值，可以直接await Promise.all();


```js
let promise1 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(1);
	},2000)
});
let promise2 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(2);
	},1000)
});
let promise3 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(3);
	},3000)
});
Promise.all([promise1,promise2,promise3]).then(res=>{
    console.log(res);
    //结果为：[1,2,3] 
})
```
调用`all`方法时的结果成功的时候是回调函数的参数也是一个数组，这个数组按顺序保存着每一个promise对象`resolve`执行时的值。


简而言之：`Promise.all( ).then( )适用于处理多个异步任务，且所有的异步任务都得到结果时的情况。`  
比如：用户点击按钮，会弹出一个弹出对话框，对话框中有两部分数据呈现，这两部分数据分别是不同的后端接口获取的数据。  
弹框弹出后的初始情况下，就让这个弹出框处于`数据加载中`的状态，当这两部分数据都从接口获取到的时候，才让这个`数据加载中`状态消失。让用户看到这两部分的数据。  
那么此时，我们就需求这两个异步接口请求任务都完成的时候做处理，所以此时，使用Promise.all方法，就可以轻松的实现，我们来看一下代码写法


##### race()
[一文详解Promise.race()方法功能及应用场景\_javascript技巧\_脚本之家](https://www.jb51.net/article/278054.htm)
`race`方法和`all`一样，接受的参数是一个每项都是`promise`的数组，但是与`all`不同的是，当最先执行完的事件执行完之后，就直接返回该`promise`对象的值。如果第一个`promise`对象状态变成`resolved`，那自身的状态变成了`resolved`；反之第一个`promise`变成`rejected`，那自身状态就会变成`rejected`。

```js
let promise1 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       reject(1);
	},2000)
});
let promise2 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(2);
	},1000)
});
let promise3 = new Promise((resolve,reject)=>{
	setTimeout(()=>{
       resolve(3);
	},3000)
});
Promise.race([promise1,promise2,promise3]).then(res=>{
	console.log(res);
	//结果：2
},rej=>{
    console.log(rej)};
)

```

**使用场景**
设置超时时间
```js
const timeout = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('Request timed out');
  }, 3000);
});
const fetchFromAPI = new Promise((resolve, reject) => {
  // 发送 API 请求
});
Promise.race([fetchFromAPI, timeout])
  .then((result) => {
    console.log(result); // 请求成功
  })
  .catch((error) => {
    console.error(error); // 请求超时
  });

```

取消异步操作
```js
const timeout = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('Request timed out');
  }, 3000);
});
const fetchFromAPI = new Promise((resolve, reject) => {
  // 发送 API 请求
});
Promise.race([fetchFromAPI, timeout])
  .then((result) => {
    console.log(result); // 请求成功
  })
  .catch((error) => {
    console.error(error); // 请求超时
  });

```


