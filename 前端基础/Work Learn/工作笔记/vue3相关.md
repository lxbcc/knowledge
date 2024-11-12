## TS提示
 
**元素隐式具有 "any" 类型，因为类型为 "string" 的表达式不能用于索引类型 "{}"。 在类型 "{}" 上找不到具有类型为 "string" 的参数的索引签名**

```tsx
interface NObject {
  [key: string]: string | number | undefined | null | void
}
```

**packageInfo.value”可能为“未定义**

```js
if (packageInfo && packageInfo.value) {
    // 安全地访问 packageInfo.value
}
```

**类型“number | undefined”的参数不能赋给类型“number”的参数**

1. 检查变量是否为`undefined`，如果是，则不传递给函数或提供一个默认值：
```js
let myNumber: number | undefined = ...;
 
if (myNumber !== undefined) {
  myFunction(myNumber); // myFunction 的参数是 number
} else {
  myFunction(defaultNumber); // 提供一个默认的 number 值
}
```

2. 使用非空断言操作符（`!`），如果你确定变量不会是`undefined`：
```js
myFunction(myNumber!); // 如果你确定myNumber不是undefined，可以使用非空断言
```

3. 使用类型断言，强制TypeScript将`number | undefined`转换为`number`：
```js
myFunction(myNumber as number); // 使用类型断言告诉TypeScript这是一个number类型
```

4. 在函数内部检查`undefined`，并提供一个默认值：
```js
function myFunction(num: number) {
  // 函数内部处理逻辑
}
 
let myNumber: number | undefined = ...;
myFunction(myNumber || 0); // 如果myNumber是undefined，则使用默认值0
```
## 知识点

[Fetching Title#7jvr](https://ts.xcatliu.com/advanced/declaration-merging.html)
[ts基本类型 typeof 和keyof\_ts typeof-CSDN博客](https://blog.csdn.net/weixin_68531033/article/details/125841511)
### typeof

```js
let a = 8
console.log(typeof a)// 'number'
```

ts中的typeof是 根据已有的值 来获取值的类型  来简化代码的书写
![[Pasted image 20240416222757.png]]

### keyof

作用：获取接口、对象（配合 typeof）、类等的所有属性名组成的联合类型。

```js
// 接口
interface Person {
    name: string
    age: number
}
type K1 = keyof Person // "name" | "age"
type K2 = keyof Person[] // "length" | "toString" | "pop" | "push" | "concat" | "join"
 
// 错误写法
const a =keyof Person
```

**要想获取对象的key组成的联合类型**
1.先获取到对象的类型 通过typeof  
2.获取对象类型所组成的联合类型

```js
const a ={name:"张三",age:18}
type keyofa =keyof typeof a
```