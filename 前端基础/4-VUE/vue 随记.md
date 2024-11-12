#### vue watch 监听不到
有的时候vue会出现这种现象，无法监听到复杂对象内部的变化：当对象里面原本有某一个属性，并对这个属性操作时，watch是可以监听到当前属性的变化的。但是，若对象里面本没有这个属性的时候，在操作时将属性添加进去，同时包括之后对这个属性的操作，watch是都检测不到的。

这是因为vue的watch会在初始化的时候通过object.defineProperty给对象的每一个属性都添加watcher来监听内部的变化。所以，后期添加上去的属性是无法检测到的。

```js
// this.$set(object, key, value)
// 使用this.$set就可以监听到
this.$set(this.obj, 'a', Math.random())
```

解决方式：

通过vue的this.$set(object, key, value)方法

通过Object.assign()重新创建一个对象, 例如this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 })


#### vue 生命周期
![[Pasted image 20230917145417.png]]
**beforeCreate** 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。在当前阶段 data、methods、computed 以及 watch 上的数据和方法都不能被访问

**created** 实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算， watch/event 事件回调。这里没有 $el, 如果非要想与 Dom 进行交互，可以通过 vm.$nextTick 来访问 Dom

**beforeMount** 在挂载开始之前被调用：相关的 render 函数首次被调用。

**mounted** 在挂载完成后发生，在当前阶段，真实的 Dom 挂载完毕，数据完成双向绑定，可以访问到 Dom 节点

**beforeUpdate** 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁（patch）之前。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程

**updated** 发生在更新完成之后，当前阶段组件 Dom 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新，该钩子在服务器端渲染期间不被调用。

**beforeDestroy** 实例销毁之前调用。在这一步，实例仍然完全可用。我们可以在这时进行善后收尾工作，比如清除计时器。

**destroyed** Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

**activated** keep-alive 专属，组件被激活时调用

**deactivated** keep-alive 专属，组件被销毁时调用

#### vue 内置指令
[Vue基本的内置指令\_vue内置指令\_小花皮猪的博客-CSDN博客](https://blog.csdn.net/weixin_46713508/article/details/130371936)

v-for v-if v-show v-bind v-on

**v-text**
v-text指令作用是用于向其所在的节点标签渲染文本内容，它与差值语法的区别在于，v-text会将节点中的内容全部替换,而差值语法可以进行拼接,会把全部的字符串当成文本去解析,不会当成标签的,哪怕写的是标签结构

**v-html**
v-html指令用于将html结构进行渲染，使用上述的v-text以及差值语法是不支持的，它只会将其通过字符的方式进行解析展示

**v-cloak**
v-cloak指令没有特定的值,不用给它指定等于号以及后面的数据,它只是一个特殊属性,等vue实例加载创建完成并接管指定的容器后,该属性会自动被剔除
类似于当页面因为网速过慢或页面加载内容过多等原因导致Vue实例无法迅速将节点内容渲染，那么页面上就会直接显示双括弧等标签，这种情况是不友好的，v-cloak属性即可解决这种问题，等渲染加载完成后再将其进行展示

**v-once**
v-once指令用于对该属性所在的标签节点在vue实例进行初次渲染后，就将其标记为静态内容，屏蔽双向绑定及其效果；后续一系列对该数据的操作都不会更新，常用于优化性能或数据标记
```html
<!-- 内置指令 -->
<div v-once>初始值{{ number }}</div> // 一直是1
<div>变动值{{ number }}</div> // 增加
<button @click="number++">点击增加</button>

number: 1

```

**v-pre**
- 这个指令可以让vue跳过其所在节点的编译过程，也就是vue不会再解析写了v-pre 的东西了
-  可利用它跳过一些代码，没有使用指令语法的节点，没有使用插值语法的节点，会更快的进行编译
```html
<h2 v-pre>vue xxxx<h2>
<h2 >{{ number }}<h2>
```


#### v-for v-if,不建议同时使用
**在使用中，v-for优先级比v-if高**
永远不要把 v-if 和 v-for 同时用在同一个元素上，带来性能方面的浪费（每次渲染都会先循环再进行条件判断）

如果避免出现这种情况，则在外层嵌套template（页面渲染不生成dom节点），在这一层进行v-if判断，然后在内部进行v-for循环
```js
<template v-if="isShow">
  <p v-for="item in items">
</template>
```

如果条件出现在循环内部，可通过计算属性computed提前过滤掉那些不需要显示的项
```javascript
computed: {
  items: function() {
   return this.list.filter(function (item) {
    return item.isShow
   })
  }
}
```

例如，使用v-for在页面中循环100个li标签，但是只显示index=97的那个li标签内容，其余的全部隐藏。  
即使100个list中只需要使用一个数据，它也会循环整个数组。

#### Vue 的父子组件生命周期钩子函数执行顺序
- 加载渲染过程
父 beforeCreate-> 父 created-> 父 beforeMount-> 子 beforeCreate-> 子 created-> 子 beforeMount-> 子 mounted-> 父 mounted

- 子组件更新过程
父 beforeUpdate-> 子 beforeUpdate-> 子 updated-> 父 updated

- 父组件更新过程
父 beforeUpdate-> 父 updated

- 销毁过程
父 beforeDestroy-> 子 beforeDestroy-> 子 destroyed-> 父 destroyed

#### 修饰符
[Vue中常用的修饰符\_vue修饰符-CSDN博客](https://blog.csdn.net/m0_64969829/article/details/122881221)
**表单修饰符**
<1> .lazy  
默认情况下，v-model同步输入框的值和数据。可以通过这个修饰符，转变为在change事件再同步。
```js
<input v-model.lazy="msg">
```
<2> .number  
自动将用户的输入值转化为数值类型
如果你先输入数字，那它就会限制你输入的只能是数字。如果你先输入字符串，那它就相当于没有加 .number 。
<3> .trim
自动过滤用户输入的首尾空格

**事件修饰符**
.stop 阻止事件继续传播
由于事件冒泡的机制，我们给元素绑定点击事件的时候，也会触发父级的点击事件。
一键阻止事件冒泡，简直方便得不行。相当于调用了 event.stopPropagation() 方法。

.prevent 阻止标签默认行为
用于阻止事件的默认行为，例如当点击提交按钮时阻止对表单的提交。相当于调用了 event.preventDefault() 方法。

.capture 使用事件捕获模式,即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理

.self 只当事件是从事件绑定的元素本身触发时才触发回调

.once 事件将只会触发一次

.passive 告诉浏览器你不想阻止事件的默认行为

**键盘事件**
```js
<input @keyup.enter="submit">      <!-- 缩写形式 -->
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

修饰键：
.ctrl
.alt
.shift
.meta

```
与按键别名不同的是，修饰键和 [keyup](https://so.csdn.net/so/search?q=keyup&spm=1001.2101.3001.7020) 事件一起用时，事件引发时必须按下正常的按键。换一种说法：如果要引发 keyup.ctrl，必须按下 ctrl 时释放其他的按键；单单释放 ctrl 不会引发事件。
```js
<!-- 按下Alt + 释放C触发 -->
<input @keyup.alt.67="clear">
 
<!-- 按下Alt + 释放任意键触发 -->
<input @keyup.alt="other">

<!-- 按下Ctrl + enter时触发 -->
<input @keydown.ctrl.13="submit">

```
**element的修饰符 (面试回答加分)**
对于elementUI的input，我们需要在后面加上.native, 因为elementUI对input进行了封装，原生的事件不起作用。
```js
<input v-model="form.name" placeholder="昵称" @keyup.enter="submit">

<el-input v-model="form.name" placeholder="昵称" @keyup.enter.native="submit"></el-input>

```