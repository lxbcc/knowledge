[Vue中 keep-alive 详解\_vue keep-alive\_明天也要努力的博客-CSDN博客](https://blog.csdn.net/ZYS10000/article/details/122480733)
[Vue之keep-alive\_vue keep-alive\_都挺好，刚刚好的博客-CSDN博客](https://blog.csdn.net/seimeii/article/details/130740391)
### 简介
keep-alive是 Vue 的内置组件，当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。
keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中

**作用**： 在组件切换过程中将状态保留在内存中，防止重复渲染DOM，减少加载时间及性能消耗，提高用户体验性。

**原理**： 在 created 函数调用时将需要缓存的 VNode 节点保存在 this.cache 中／在 render（页面渲染） 时，如果 VNode 的 name 符合缓存条件（可以用 include 以及 exclude 控制），则会从 this.cache 中取出之前缓存的 VNode 实例进行渲染。
（VNode：虚拟DOM，其实就是一个JS对象）

### 2. 使用

#### 2.1 参数
1. include：匹配的路由/组件会被缓存
2. exclude：匹配的路由/组件不会被缓存
3. max：最大缓存数

**注意: include/exclude 值是组件中的 name 命名，而不是路由中的组件 name 命名；**

```js
// App.vue
<keep-alive include="test">
   <router-view/>
</keep-alive>

--------------------------------------------------------------------------------------------
补充： include/exclude 值的多种形式。

// 1. 将缓存 name 为 test 的组件(基本）
<keep-alive include='test'>
  <router-view/>
</keep-alive>
	
// 2. 将缓存 name 为 a 或者 b 的组件，结合动态组件使用
<keep-alive include='a,b'>
  <router-view/>
</keep-alive>
	
// 3. 使用正则表达式，需使用 v-bind
<keep-alive :include='/a|b/'>
  <router-view/>
</keep-alive>	
	
// 4.动态判断
<keep-alive :include='includedComponents'>
  <router-view/>
</keep-alive>
	
// 5. 将不缓存 name 为 test 的组件
<keep-alive exclude='test'>
  <router-view/>
</keep-alive>

// 6. 和 `<transition>` 一起使用
<transition>
  <keep-alive>
    <router-view/>
  </keep-alive>
</transition>

// 7. 数组 (使用 `v-bind`)
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>

```


#### 2.2 生命周期函数
activated
deactivated

使用< keep-alive > 会将数据保留在内存中，如果要在每次进入页面的时候获取最新的数据，需要在 activated 阶段获取数据，承担原来created钩子中获取数据的任务。
被包含在 < keep-alive > 中创建的组件，会多出两个生命周期的钩子: activated 与 deactivated
activated：在组件被激活时调用，在组件第一次渲染时也会被调用，之后每次keep-alive激活时被调用。
deactivated：在组件被停用时调用。
**注意：** 只有组件被 keep-alive 包裹时，这两个生命周期才会被调用，如果作为正常组件使用，是不会被调用，以及在 2.1.0 版本之后，使用 exclude 排除之后，就算被包裹在 keep-alive 中，这两个钩子依然不会被调用！另外在服务端渲染时此钩子也不会被调用的。


什么时候获取数据？

当引入keep-alive 的时候，页面第一次进入，钩子的触发顺序 created -> mounted -> activated，退出时触发 deactivated。
当再次进入（前进或者后退）时，只触发 activated。

我们知道 keep-alive 之后页面模板第一次初始化解析变成 HTML 片段后，再次进入就不在重新解析而是读取内存中的数据。
只有当数据变化时，才使用 VirtualDOM 进行 diff 更新。所以，页面进入的数据获取应该在 activated 中也放一份。
数据下载完毕手动操作 DOM 的部分也应该在activated中执行才会生效。

所以，应该 activated 中留一份数据获取的代码，或者不要 created 部分，直接将 created 中的代码转移到 activated 中。




#### 登录
##### 账号密码登录
```js
<el-input v-model.trim="signin.login" auto-complete="off" :placeholder="$t('auth.signin.login_placeholder')" />
<el-input v-model.trim="signin.password" type="password" auto-complete="off" :placeholder="$t('auth.signin.passowrd_placeholder')" @keyup.native.enter="submit" />
```
表单校验 发送请求
请求成功之后设置cookie
```js
setUserInfo(res) {
  this.$cookie.set('ms-sid', res.session_id, 1)
  this.$cookie.set('id-num', res.profile.id, 1)
  this.$cookie.set('ms-uuid', this.uuid, 1)
  this.$router.push('/dashboard')
},
```

##### 扫码登录
请求获取二维码 base64图片
定时器轮询，是否已扫码的接口
	根据状态判断是否已经扫码成功
	成功进入首页
	待确认状态
	报错超时，取消轮询

退出
global.signout
```
Global.prototype.signout = function signout() {
  Vue.cookie.remove('ms-sid')
  Vue.cookie.remove('id-num')
  Vue.cookie.remove('ms-uuid')
  Vue.cookie.remove('index')
  setTimeout(() => {
    location.reload()
  }, 300)
}
```


