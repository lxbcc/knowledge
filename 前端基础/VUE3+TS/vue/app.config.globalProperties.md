
一个用于注册能够被应用内所有组件实例访问到的全局属性的对象。

类型
```ts
interface AppConfig {
  globalProperties: Record<string, any>
}
```

这是对 Vue 2 中 `Vue.prototype` 使用方式的一种替代，此写法在 Vue 3 已经不存在了。与任何全局的东西一样，应该谨慎使用。

用法
```js
app.config.globalProperties.msg = 'hello'
```

这使得 `msg` 在应用的任意组件模板上都可用，并且也可以通过任意组件实例的 `this` 访问到：

```js
export default {
  mounted() {
    console.log(this.msg) // 'hello'
  }
}
```