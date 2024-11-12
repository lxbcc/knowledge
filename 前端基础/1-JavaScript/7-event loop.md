重点[面试必问之 JS 事件循环(Event Loop)，看这一篇足够 - 知乎](https://zhuanlan.zhihu.com/p/580956436)
[JS事件循环机制（event loop）之宏任务/微任务 - 掘金](https://juejin.cn/post/6844903638238756878)
重点[Eventloop不可怕，可怕的是遇上Promise - 掘金](https://juejin.cn/post/6844903808200343559)
[【js进阶】全面理解Event Loop这一篇就够了\_js evenloop\_goodlovingz的博客-CSDN博客](https://blog.csdn.net/qq_31967985/article/details/110310685)
[js的单线程事件循环机制(event loop)的典型例子\_js时间循环机制例题\_吃瓜群众欢乐多的博客-CSDN博客](https://blog.csdn.net/sinat_36146776/article/details/89397654)


### 浏览器 JS 异步执行的原理

浏览器是多线程的，当 JS 需要执行异步任务时，浏览器会另外启动一个线程去执行该任务。也就是说，“JS 是单线程的”指的是执行 JS 代码的线程只有一个，是浏览器提供的 JS 引擎线程（主线程）。浏览器中还有定时器线程和 HTTP 请求线程等，这些线程主要不是来跑 JS 代码的。

比如主线程中需要发一个 AJAX 请求，就把这个任务交给另一个浏览器线程（HTTP 请求线程）去真正发送请求，待请求回来了，再将 callback 里需要执行的 JS 回调交给 JS 引擎线程去执行。**即浏览器才是真正执行发送请求这个任务的角色，而 JS 只是负责执行最后的回调处理。** 所以这里的异步不是 JS 自身实现的，其实是浏览器为其提供的能力。

**作为前端开发者，主要重点关注其渲染进程，渲染进程下包含了 JS 引擎线程、HTTP 请求线程和定时器线程等，这些线程为 JS 在浏览器中完成异步任务提供了基础。**

### 浏览器中的事件循环
#### 执行栈与任务队列
![[Pasted image 20231003215657.png]]
#### 宏任务微任务
任务队列不只一个，根据任务的种类不同，可以分为微任务（micro task）队列和宏任务（macro task）队列。

事件循环的过程中，执行栈在同步代码执行完成后，优先检查微任务队列是否有任务需要执行，如果没有，再去宏任务队列检查是否有任务执行，如此往复。微任务一般在当前循环就会优先执行，而宏任务会等到下一次循环，**因此，微任务一般比宏任务先执行，并且微任务队列只有一个，宏任务队列可能有多个**。另外我们常见的点击和键盘等事件也属于宏任务。

下面我们看一下常见宏任务和常见微任务。

**常见宏任务：**

- setTimeout()
- setInterval()
- setImmediate()

**常见微任务：**

- promise.then()、promise.catch()
- new MutaionObserver()
- process.nextTick()

我们再来分析下 promise.then（微任务）的处理。当执行到 promise.then 时，V8 引擎不会将异步任务交给浏览器其他线程，而是将回调存在自己的一个队列中，待当前执行栈执行完成后，立马去执行 promise.then 存放的队列，promise.then 微任务没有多线程参与，甚至从某些角度说，微任务都不能完全算是异步，它只是将书写时的代码修改了执行顺序而已。

[setTimeout和setImmediate到底谁先执行，本文让你彻底理解Event Loop - \_蒋鹏飞 - 博客园](https://www.cnblogs.com/dennisj/p/12550996.html)

#### 题

```js
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})

```

```js
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    return new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})

```

```js
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})
new Promise((resolve,reject)=>{
    console.log("promise3")
    resolve()
}).then(()=>{
    console.log("then31")
})

```

```js
new Promise((resolve,reject)=>{
  console.log("promise1")
  resolve()
}).then(()=>{
  console.log("then11")
  new Promise((resolve,reject)=>{
      console.log("promise2")
      resolve()
  }).then(()=>{
      console.log("then21")
  }).then(()=>{
      console.log("then23")
  })
}).then(()=>{
  console.log("then12")
})
new Promise((resolve,reject)=>{
  console.log("promise3")
  resolve()
}).then(()=>{
  console.log("then31")
}).then(()=> {
  console.log('then32')
})
```