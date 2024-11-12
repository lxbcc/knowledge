[vue中nextTick()的理解及使用\_vue nexttick\_司徒小北的博客-CSDN博客](https://blog.csdn.net/weixin_42333548/article/details/102606546)
### $nextTick的定义及理解
**定义**：在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
所以就衍生出了这个获取更新后的DOM的Vue方法。所以放在Vue.nextTick()[回调函数](https://so.csdn.net/so/search?q=%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0&spm=1001.2101.3001.7020)中的执行的应该是会对DOM进行操作的 js代码；
**理解**：nextTick()，是将回调函数延迟在下一次dom更新数据后调用，简单的理解是：当数据更新了，在dom中渲染后，自动执行该函数

### $nextTick(callback) 使用原理
Vue是异步执行dom更新的，一旦观察到数据变化，Vue就会开启一个队列，然后把在同一个事件循环 (event loop) 当中观察到数据变化的 watcher 推送进这个队列。如果这个watcher被触发多次，只会被推送到队列一次。这种缓冲行为可以有效的去掉重复数据造成的不必要的计算和Dom操作。而在下一个事件循环时，Vue会清空队列，并进行必要的DOM更新。
当你设置 改变了一个新数据data，DOM 并不会马上更新，而是在异步队列被清除，也就是下一个事件循环开始时执行更新时才会进行必要的DOM更新。如果此时你想要根据更新的 DOM 状态去做某些事情，就会出现问题。。为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用 vue.nextTick(callback) 。这样回调函数在 DOM 更新完成后就会调用。

### 什么时候需要用的this.nextTick()

Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中，原因是在created()钩子函数执行的时候DOM 其实并未进行任何渲染，而此时进行DOM操作无异于徒劳，所以此处一定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。与之对应的就是mounted钩子函数，因为该钩子函数执行时所有的DOM挂载已完成。

当项目中你想在改变DOM元素的数据后基于新的dom做点什么，对新DOM一系列的js操作都需要放进Vue.nextTick()的回调函数中；通俗的理解是：更改数据后当你想立即使用js操作新的视图的时候需要使用它
