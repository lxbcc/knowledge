#### MVVM模型
![[Pasted image 20230826192735.png]]
View是视图，就是DOM；对应视图也就是HTML部分--代表UI组件，它负责将数据模型转化成UI展现出来。 Model是模型，就是vue组件里的data，或者说是vuex里的数据；--代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。 ViewModel--监听模型数据也就是data的的改变和控制视图行为、处理用户交互，简单理解就是一个同步View和Model的对象，连接Model和View。

虽然没有完全遵循 [MVVM 模型](https://zh.wikipedia.org/wiki/MVVM)，但是 Vue 的设计也受到了它的启发。因此在文档中经常会使用 `vm` (ViewModel 的缩写) 这个变量名表示 Vue 实例。



#### Vue基本原理
[十分钟浅入Vue 原理 - 知乎](https://zhuanlan.zhihu.com/p/138114429)
[Vue响应式原理-理解Observer、Dep、Watcher - 掘金](https://juejin.cn/post/6844903858850758670?searchId=20230826172952741FB8257656F157C0A2)
[10分钟读懂vue数据响应式和双向绑定原理 - 掘金](https://juejin.cn/post/7133096500749565960?searchId=20230826172952741FB8257656F157C0A2)
`Vue`响应式原理的核心就是`Observer`、`Dep`、`Watcher`。

通过Object.defineProperty()来劫持对象属性的setter和getter操作，在数据变动时做你想要做的事情

**Object.defineProperty的缺点**
深度监听需要递归到底，一次性计算量大
无法监听新增属性、删除属性（要使用Vue.set Vue.delete）
无法原生监听数组，需要特殊处理

利用ES5中的Object.defineProperty结合观察者模式实现的，然后利用里面的getter和setter来实现双向数据绑定的、发布订阅模式，数据劫持

首先要对数据进行劫持监听，设置一个监听器Observer，用来监听所有属性。如果属性发上变化了，就需要告诉订阅者Watcher看是否需要更新。因为订阅者是有很多个，所以我们需要有一个消息订阅器Dep来专门收集这些订阅者，然后在监听器Observer和订阅者Watcher之间进行统一管理的。接着，我们还需要有一个指令解析器Compile，对每个节点元素进行扫描和解析，将相关指令（如v-model，v-on）对应初始化成一个订阅者Watcher，并替换模板数据或者绑定相应的函数，此时当订阅者Watcher接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。

#### 双向数据绑定的原理

Vue.js 是采用**数据劫持**结合**发布者-订阅者模式**的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤：

1. 需要observe的数据对象进行递归遍历，包括子属性对象的属性，都加上setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
2. compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
3. Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是: ①在自身实例化时往属性订阅器(dep)里面添加自己 ②自身必须有一个update()方法 ③待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。
4. MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。



#### 双向绑定
双向绑定的作用是：`数据和视图相互驱动更新，是相互影响的关系`

##### v-model属性
vue中一般通过`v-model`体现双向绑定。
`v-model`实际上是`v-on`和`v-bind`的语法糖。
```vue
<input type="text" :value="msg" v-on:input="msg=$event.target.value">
```
针对于input的`v-model`双向数据绑定实际上就是通过子组件中的`$emit`方法派发`input`事件，父组件监听`input`事件中传递的值，并存储在父组件`data`中；然后父组件再通过`prop`的形式传递给子组件,在子组件中绑定`input`的`value`属性。

其他元素使用`v-model`双向数据绑定实际上就是，通过监听`change`事件。以及`$emit`方法派发，再通过`prop`的形式传递。


#### 虚拟dom
虚拟DOM就是为了解决浏览器性能问题而被设计出来的
- patch(containerro/容器, vnode/虚拟dom) ,表示把vnode渲染到DOM结构中
- patch(vnode, newVnode); 表示更新已有的内容（具体怎么计算更新对应的内容就是使用`diff`算法）

##### **初次渲染过程**
- 解析模板为`render`函数（一般在开发环境已经完成，vue-loader）4
- 触发响应式，监听data属性`getter``setter`(模板中使用到的变量会触发`getter`)
- 执行`render`函数（触发`getter`），生成vnode，`patch(elem,vnode)`渲染到页面上
注意如果模板中没有用的data数据就不会触发`getter`,因为和视图没关系（vue里面的优化）

 **更新过程**

- 修改data的数据，触发`setter`（此前data数据在`getter`中已被监听）
- 重新执行`render`函数，生成`newVnode(新的虚拟dom)`
- 使用 `patch(vnode,newVnode)`更新到页面上

#### 为什么data是一个函数
组件中的 data 写成一个函数，数据以函数返回值形式定义，这样每复用一次组件，就会返回一份新的 data，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例共用了一份 data，就会造成一个变了全都会变的结果