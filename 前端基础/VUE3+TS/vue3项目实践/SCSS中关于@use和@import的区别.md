[SCSS中关于@use和@import的区别 - 掘金](https://juejin.cn/post/7258495816732852279)
## 区别

|功能/特性|@import|@use|
|---|---|---|
|作用域|共享同一个作用域|创建命名空间，具有隔离性|
|重复加载|可能导致重复加载|保证每个模块只加载一次|
|推荐版本|较旧版本的导入方式|新版本 Sass 推荐的导入方式|
|命名空间|无|可以自定义命名空间|
|模块化支持|较弱|提供更好的模块化支持|
|性能|可能存在性能问题|更优化的性能|
|避免全局污染|不提供隔离性|提供隔离性|
|导入方式|使用 @import 导入|使用 @use 导入|

## 重复加载

当使用 `@import` 导入模块时，如果在多个文件中多次导入同一个文件，可能会导致重复加载的问题。这意味着被导入的文件将在每个使用了 `@import` 的文件中都被加载一次，导致样式表中包含多份相同的样式，从而影响性能和增加文件大小。

## 命名空间

@import是没有命名空间的，我们来介绍一下@use的命名空间是如何作用的。

### 1. 不使用as,则直接将文件名当作命名空间

在 SCSS 中，当在 `@use` 后面直接跟上文件路径，并且没有使用 `as` 关键字指定命名空间，表示将导入的模块
内容整体作为一个命名空间，并且使用被导入文件的名称作为命名空间的标识。

```js
// _variables.scss

$primary-color: #007bff;
$secondary-color: #6c757d;
```

```js
// styles.scss

@use 'variables.scss';
body {
  background-color: variables.$primary-color;
}

button {
  background-color: variables.$secondary-color;
}
```
因为 `@use` 创建了命名空间，所以我们在 `styles.scss` 中需要使用 `variables.$primary-color` 和 `variables.$secondary-color` 来访问 `_variables.scss` 中定义的变量

### 2. 使用as xxx，则以后面命名的变量xxx作为命名空间

```js
// styles.scss
@use 'variables.scss' as customVars;

body {
  background-color: customVars.$primary-color;
}

button {
  background-color: customVars.$secondary-color;
}
```

在这个示例中，`styles.scss` 使用 `@use` 导入了 `_variables.scss` 文件，并使用 `as customVars` 为导入的模块创建了一个命名空间 `customVars`。然后，我们使用 `+` 后面加上 `customVars` 来指定变量名作为命名空间的标识。

在 `styles.scss` 中，我们可以通过 `customVars.$primary-color` 和 `customVars.$secondary-color` 来访问 `_variables.scss` 中定义的变量。这样的用法可以避免变量冲突，并提供更清晰的命名空间标识，增加代码的可读性。


### 通过 as * 来导入

如果在 `@use` 后面使用 `as *`，表示将导入的模块的所有内容直接合并到当前文件中，并且不会创建一个命名空间。这样可以让导入的模块的所有变量、mixin、函数等直接在当前文件中使用，而不需要使用命名空间来访问它们。
```js
// styles.scss

@use 'variables.scss' as *;

body {
  background-color: $primary-color;
}

button {
  background-color: $secondary-color;
}
```