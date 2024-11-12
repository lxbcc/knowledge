## 动态绑定class

### 对象语法

v-bind:class 是VUE中用来动态绑定CSS类的指令。可以根据Vue实例中的数据来动态添加或移除HTML元素的类。这样可以根据数据的变化来动态改变元素的样式，实现更灵活的样式控制。

```vue
<div v-bind:class="{ 'class-name': condition }"></div>
```

- <mark class="hltr-cyan">class-name</mark>: 要绑定的CSS类名
- <mark class="hltr-cyan">condition</mark>: 一个表达式，<mark class="hltr-yellow">当为true时，class-name会被添加；当为false时，class-name会被移除</mark>。

```vue
<template>
  <div v-bind:class="{ active: isActive, error: hasError }">
    <!-- 内容 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true, // 根据条件决定是否添加active类
      hasError: false, // 根据条件决定是否添加error类
    };
  },
};
</script>

<style>
.active {
  color: blue;
}

.error {
  color: red;
}
</style>
```

绑定的数据对象不必内联定义在模板里：

```js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

也可以绑定一个返回对象的计算属性

```vue
<div v-bind:class="classObject"></div>
```

```js
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### 数组语法

```vue
<div v-bind:class="[classA, classB]"></div>
```

<mark class="hltr-cyan">classA, classB</mark>: 字符串，表示要绑定的CSS类名。可以是变量、对象属性或直接的类名字符串。

```vue
<template>
  <div v-bind:class="[activeClass, errorClass]">
    <!-- 内容 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: true,
      hasError: false,
    };
  },
  computed: {
    activeClass() {
      return this.isActive ? 'active' : '';
    },
    errorClass() {
      return this.hasError ? 'error' : '';
    },
  },
};
</script>

<style>
.active {
  color: blue;
}

.error {
  color: red;
}
</style>

```

如果你也想根据条件切换列表中的 class，可以用三元表达式：

```vue
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

在数组语法中也可以使用对象语法

```vue
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```


### 用在组件上

当在一个自定义组件上使用 `class` property 时，这些 class 将被添加到该组件的根元素上面。这个元素上已经存在的 class 不会被覆盖。

```js
Vue.component('my-component', {  
	template: '<p class="foo bar">Hi</p>'  
})

<my-component class="baz boo"></my-component>
// HTML 将被渲染为：
<p class="foo bar baz boo">Hi</p>

// 对于带数据绑定 class 也同样适用：
<my-component v-bind:class="{ active: isActive }"></my-component>
// 当 isActive 为 truthy 时，HTML 将被渲染成为：
<p class="foo bar active">Hi</p>

```

## 绑定内联样式

### 对象语法

```vue
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
data: {
  activeColor: 'red',
  fontSize: 30
}
```

直接绑定到一个样式对象通常更好，这会让模板更清晰：

```js
<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
同样的，对象语法常常结合返回对象的计算属性使用。

### 数组语法

```js
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```