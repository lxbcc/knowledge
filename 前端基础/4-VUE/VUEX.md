#### Vuex 的原理
![[Pasted image 20230827145722.png]]

为Vue Components建立起了一个完整的生态圈，包括开发中的API调用一环。 
**（1）核心流程中的主要功能：**
- Vue Components 是 vue 组件, 组件会触发（dispatch）一些事件或动作，也就是图中的 Actions;

- 在组件中发出的动作，肯定是想获取或者改变数据的, 但是在 vuex 中，数据是集中管理的，不能直接去更改数据，所以会把这个动作提交（Commit）到 Mutations 中;

- 然后 Mutations 就去改变（Mutate）State 中的数据;

- 当 State 中的数据被改变之后，就会重新渲染（Render）到 Vue Components 中去，组件展示更新后的数据，完成一个流程。

**（2）各模块在核心流程中的主要功能：**
- `Vue Components`∶ Vue组件。HTML页面上，负责接收用户操作等交互行为，执行dispatch方法触发对应action进行回应。
- `dispatch`∶操作行为触发方法，是唯一能执行action的方法。
- `actions`∶ 操作行为处理模块。负责处理Vue Components接收到的所有交互行为。包含同步/异步操作，支持多个同名方法，按照注册的顺序依次触发。向后台API请求的操作就在这个模块中进行，包括触发其他action以及提交mutation的操作。该模块提供了Promise的封装，以支持action的链式触发。
- `commit`∶状态改变提交操作方法。对mutation进行提交，是唯一能执行mutation的方法。
- `mutations`∶状态改变操作方法。是Vuex修改state的唯一推荐方法，其他修改方式在严格模式下将会报错。该方法只能进行同步操作，且方法名只能全局唯一。操作之中会有一些hook暴露出来，以进行state的监控等。
- `state`∶ 页面状态管理容器对象。集中存储Vuecomponents中data对象的零散数据，全局唯一，以进行统一的状态管理。页面显示所需的数据从该对象中进行读取，利用Vue的细粒度数据响应机制来进行高效的状态更新。
- `getters`∶ state对象读取方法。图中没有单独列出该模块，应该被包含在了render中，Vue Components通过该方法读取全局state对象。


####  Vuex中action和mutation的区别
mutation中的操作是一系列的同步函数，用于修改state中的变量的的状态。当使用vuex时需要通过commit来提交需要操作的内容。mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      state.count++      // 变更状态
    }
  }
})
```

当触发一个类型为 increment 的 mutation 时，需要调用此函数
```js
store.commit('increment')
```

而Action类似于mutation，不同点在于：

- Action 可以包含任意异步操作。
- Action 提交的是 mutation，而不是直接变更状态。

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```
Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters。 所以，两者的不同点如下：

- Mutation 专注于修改State，理论上是修改State的唯一途径；Action业务代码、异步请求。
- Mutation：必须同步执行；Action：可以异步，但不能直接操作State。
- 在视图更新时，先触发actions，actions再触发mutation
- mutation的参数是state，它包含store中的数据；store的参数是context，它是 state 的父级，包含 state、getters


#### Vuex有哪几种属性？

有五种，分别是 State、 Getter、Mutation 、Action、 Module

- state => 基本数据(数据源存放地)
- getters => 从基本数据派生出来的数据
- mutations => 提交更改数据的方法，同步
- actions => 像一个装饰器，包裹mutations，使之可以异步。
- modules => 模块化Vuex

#### 如何在组件中批量使用Vuex的getter属性

使用mapGetters辅助函数, 利用对象展开运算符将getter混入computed 对象中
```js
import {mapGetters} from 'vuex'
export default{
    computed:{
        ...mapGetters(['total','discountTotal'])
    }
}
```



#### 代码
**index.js**
```js
Vue.use(Vuex)
const store = new Vuex.Store({
	modules: {
		app,
		address,
		courier,
		dict,
		count,
		franchisee,
		kaclientgrade, // 特殊价格表
		route
	},
	getters

})
```


```js
const state = {
	dict: {}, // 静态资源key
	asset: {} // 静态资源文本
}

const mutations = {
	SET_ADDRESS_LIST: (state, { dict, asset }) => {
		state.dict = dict
		state.asset = asset
	}
}

const actions = {
	changeDict({ commit }, { dict, asset }) {
		commit('SET_ADDRESS_LIST', { dict: dict, asset: asset })
	}
}

export default {
	namespaced: true,
	state,
	mutations,
	actions
}
```