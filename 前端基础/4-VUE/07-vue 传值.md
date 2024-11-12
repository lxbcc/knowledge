[vue中组件之间传值的六种方式（完整版）\_vue 组件传值-CSDN博客](https://blog.csdn.net/m0_62876802/article/details/125783872)
#### props/$emit
父组件通过 `props` 向下传递数据给子组件
子组件通过 `events` 给父组件发送消息，实际上就是子组件把自己的数据发送到父组件

#### bus/on
##### 自己写
首先，先创建bus.js文件，然后在引用，这里有两种引用方法：局部引用和全局引用。
```js
// bus.js文件内容
import Vue from 'vue'
// export default new Vue()
const Bus = new Vue()
export default Bus
```

全局引用
```js
import Bus from '../src/bus' //这是我的路径，正确引用你们的路径
Vue.prototype.$bus = Bus 
```

局部
```js
import eventBus from './EventBus'
eventBus.$emit("busEvent", this.message);
```

父组件
```js
<template> 
	<div>
		<bus-learn></bus-learn>
		<bus-test></bus-test>
	</div>
</template>
```

```js
子组件1
this.$bus.$emit("busEvent", this.message)
子组件2
this.$bus.$on("busEvent", (mes) => { this.message = mes; });
```


vue-bus
```js
import VueBus from 'vue-bus'
Vue.use(VueBus);`
```

#### $ref
```js
<Children ref="children"></Children>
this.$refs.children.updateMsg('Have you received the clothes？')
```

#### 依赖注入provide inject
[Vue中 provide、inject 详解及使用\_vue provide inject用法-CSDN博客](https://blog.csdn.net/ZYS10000/article/details/123243486)
1.概念

成对出现：provide和inject是成对出现的
　　作用：用于父组件向子孙组件传递数据
　　使用方法：provide在父组件中返回要传给下级的数据，inject在需要使用这个数据的子辈组件或者孙辈等下级组件中注入数据。
　　使用场景：由于vue有$parent属性可以让子组件访问父组件。但孙组件想要访问祖先组件就比较困难。通过provide/inject可以轻松实现跨级访问父组件的数据
2.简单来说
　　provider/inject：简单的来说就是在父组件中通过provider来提供变量，然后在子组件中通过inject来注入变量
　　需要注意的是这里不论子组件有多深，只要调用了inject那么就可以注入provider中的数据。而不是局限于只能从当前父组件的prop属性来获取数据。
##### 使用

```js
provide：Object | () => Object
inject： Array<string> | { [key: string]: string | Symbol | Object }
```
**provide**
对象形式，不能访问data
```js
export default {
  provide: {
    message: 'hello!'
  }
}
```

如果要提供依赖当前组件实例的状态 (比如那些由 `data()` 定义的数据属性)，可以以函数形式使用 `provide`
不是响应式的
```js
export default {
  data() {
    return {
      message: 'hello!'
    }
  },
  provide() {
    // 使用函数的形式，可以访问到 `this`
    return {
      message: this.message
    }
  }
}
```

**inject**
```js
export default {
  inject: ['message'],
  created() {
    console.log(this.message) // injected value
  }
}
```

注入会在组件自身的状态**之前**被解析，因此你可以在 `data()` 中访问到注入的属性：

```js
export default {
  inject: ['message'],
  data() {
    return {
      // 基于注入值的初始数据
      fullMessage: this.message
    }
  }
}
```

注入别名
```js
export default {
  inject: {
    /* 本地属性名 */ localMessage: {
      from: /* 注入来源名 */ 'message'
    }
  }
}
```
注入默认值
```js
export default {
  // 当声明注入的默认值时
  // 必须使用对象形式
  inject: {
    message: {
      from: 'message', // 当与原注入名同名时，这个属性是可选的
      default: 'default value'
    },
    user: {
      // 对于非基础类型数据，如果创建开销比较大，或是需要确保每个组件实例
      // 需要独立数据的，请使用工厂函数
      default: () => ({ name: 'John' })
    }
  }
}
```


响应式
**provide 和 inject 绑定并不是可响应的。这是刻意为之的。然而，如果你传入了一个可监听的对象，那么其对象的 property 还是可响应的。**
```js
  provide(){
    return {
      obj: this.obj, // 方式1.传入一个可监听的对象
      developerFn:() => this.developer, // 方式2.通过 computed 来计算注入的值
      year: this.year, // 方式3.直接传值
      app: this, // 4. 提供祖先组件的实例 缺点：实例上挂载很多没有必要的东西 比如：props，methods。
    }
  }
```
总结
**慎用 provide / inject**
provide/inject 明显破坏了单向数据流原则。试想，如果有多个后代组件同时依赖于一个祖先组件提供的状态，那么只要有一个组件修改了该状态，那么所有组件都会受到影响。这一方面增加了耦合度，另一方面，使得数据变化不可控。如果在多人协作开发中，这将成为一个噩梦
**使用 provide / inject 编写组件**
使用 provide/inject 做组件开发，是 Vue 官方文档中提倡的一种做法。
#### $parent $children
通过parent 可以获父组件实例，然后通过这个实例就可以访问父组件的属性和方法，它还有一个兄弟 parent可以获父组件实例，然后通过这个实例就可以访问父组件的属性和方法，它还有一个兄弟parent可以获父组件实例，然后通过这个实例就可以访问父组件的属性和方法，它还有一个兄弟root，可以获取根组件实例。
```js
// 获父组件的数据
this.$parent.foo

// 写入父组件的数据
this.$parent.foo = 2

// 访问父组件的计算属性
this.$parent.bar

// 调用父组件的方法
this.$parent.baz()
```

```js
handlerMe(){
	console.log('==>',this.$children) //输出数组长度为1了 
	this.$children[0].msg = '我改了数据'
}
```
#### $attrs / $listeners
[vue的$listeners的使用\_vue的listeners-CSDN博客](https://blog.csdn.net/formylovetm/article/details/126046938)
多级组件嵌套需要传递数据时，通常使用的方法是通过 **vuex**。但如果仅仅是传递数据，而不做中间处理，使用 **vuex** 处理，未免有点大材小用

`$attrs`：包含了父作用域中不被 `prop` 所识别 (且获取) 的特性绑定 (**class** 和 **style** 除外)。当一个组件没有声明任何 `prop` 时，这里会包含所有父作用域的绑定 (**class** 和 **style** 除外)，并且可以通过 `v-bind="$attrs"` 传入内部组件。通常配合 **interitAttrs** 选项一起使用。
```js
<child-com2 v-bind="$attrs"></child-com2>
```

子组件使用inheritAttrs = true，那么特性显示在dom上，如果设置为false，那么特不显示在dom上
![[Pasted image 20230929103722.png]]

- `$listeners`：包含了父作用域中的 (不含 `.native` 修饰器的) `v-on` 事件监听器。它可以通过 `v-on="$listeners"` 传入内部组件。
[Vue中 $attrs、$listeners 详解及使用\_明天也要努力的博客-CSDN博客](https://blog.csdn.net/ZYS10000/article/details/116017711)
