[初识ES6 Proxy属性 - 掘金](https://juejin.cn/post/6844903984478568462?from=search-suggest)

Proxy 是ES6引入的一个元编程特性，它允许你创建一个代理对象，用于拦截并自定义JavaScript对象的基本操作。通过代理对象，Proxy的核心思想是在目标对象和代码之间建立一个拦截层，使得可以对目标对象的操作进行拦截和监视。

### 创建Proxy对象

```js
let proxy = new Proxy(target, handler)
```

- `target`: 要使用proxy包装的`目标对象`（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。
- `handler`: 一个对象，定义了代理对象的行为，包括捕获器(handlers)等

### Proxy的Handler对象
Handler对象是一个包含了代理行为的集合。它定义了当在代理对象上进行操作时应该如何响应这些操作。Handler对象可以定义各种捕获器（handlers），这些捕获器对应了不同的操作。一些常见的捕获器有：
- `get(target, property, receiver)`：拦截对象属性的读取操作。
- `set(target, property, value, receiver)`：拦截对象属性的设置操作。
- `has(target, property)`：拦截`in`操作符。
- `deleteProperty(target, property)`：拦截`delete`操作符。
- `apply(target, thisArg, argumentsList)`：拦截函数的调用。
- `construct(target, argumentsList, newTarget)`：拦截`new`操作符。



### handler.get()
含义：拦截对象的读取
语法
```js
var p = new Proxy(target, {
  get: function(target, property, receiver) {
  }
});

```

`target`: 目标对象（函数）。
`property`: 被获取的属性名。
`receiver`: Proxy或者继承Proxy的对象。
`返回值`：get 方法可以返回任何值
`拦截目标对象`：
- 访问属性：proxy[foo] 和 proxy.bar
- 访问原型链上的属性：Object.create(proxy)[foo]
- Reflect.get()

示例
```js
// 1
const proxy1 = new Proxy({
  a: 1
}, {
  get(target, key) {
    if(Reflect.has(target,key)) {
      return Reflect.get(target,key);
    }else {
        return false;
    }
    
  }
})
proxy1.a //1
proxy1.b //false
```

```js
// 2
var p = new Proxy({}, {
    get: function(target, prop, receiver) {
        console.log('target', target) // {}
        console.log('called: ' + prop) // "called: a“
        console.log('receiver', receiver) // Proxy {}
        
        return 10
    }
})

console.log(p.a) // 10

---

var obj = {}
Object.defineProperty(obj, 'a', {
    configurable: false,
    enumerable: false,
    value: 10,
    writable: false
})

var p = new Proxy(obj, {
    get: function(target, prop) {
        return 20
    }
})
console.log(p.a) // 会抛出 TypeError 'get'上的代理:属性'a'是一个只读的和不可配置的数据属性
```

### handler.set

含义：拦截对象的设置属性操作
语法：
```js
var p = new Proxy(target, {
  set: function(target, property, value, receiver) {}
});
```

`target`：要代理的目标对象。  
`property`：要设置的属性名。  
`value`：要设置的属性值。  
`receiver`：proxy实例（可选参数，一般不用）
`返回值`：set方法应当返回一个布尔值
- 返回true代表设置属性成功
- 在严格模式下，set方法返回false，那么会抛出一个TypeError异常
`拦截目标对象`：
- 访问属性：proxy[foo] = bar 和 proxy.bar = foo
- 访问原型链上的属性：Object.create(proxy)[foo] = bar
- Reflect.set()
示例
```js
const monster1 = { eyeCount: 4 }

const handler1 = {
    set(obj, prop, value) {
        if ((prop === 'eyeCount') && ((value % 2) !== 0)) {
            console.log('Monsters must have an even numbers of eyes')
        } else {
            return Reflect.set(...arguments)
        }
    }
}

const proxy1 = new Proxy(monster1, handler1)

proxy1.eyeCount = 1 // expected Output: "Monsters must have an even numbers of eyes"
console.log(proxy1.eyeCount); // expected Output: 4

proxy1.eyeCount = 2
console.log(proxy1.eyeCount) // expected output: 2

```

### has

含义：判断对象是否具有某个属性。

语法
```js
const p = new Proxy(target,{
	has: function(target, key) {}
})
```

`target`：要代理的目标对象。  
`key`：要设置的属性名。
```js
const handler = {
	has(target, key) {
			if (key[0] === '_') {
					console.log('it is a private property')
					return false;
			}
			return key in target;
	}
};
const target = {
	_a: 123,
	a: 456
};
const proxy = new Proxy(target, handler);
console.log('_a' in proxy) // it is a private property  false
console.log('a' in proxy);//true
```
当对proxy使用`in`运算符时，就会自动触发has方法，如果判断当前属性以_开头的话，就进行拦截，从而不被后面的`in`运算符发现，实现隐藏某些特定属性的目的。
### handler.apply

含义：拦截函数的调用

语法
```js
var p = new Proxy(target, {
  apply: function(target, thisArg, argumentsList) {
  }
});
```

**target**: 目标对象（函数）。
**thisArg**：被调用时的上下文对象。
**argumentsList**: 被调用时的参数数组。
**返回值**：apply 方法可以返回任何值。
**拦截目标对象**：

- proxy(...args)
- Function.prototype.apply() 和 Function.prototype.call()
- Reflect.apply()
```js
function sum(a, b) {
    return a + b
}

const handler = {
    apply: function(target, thisArg, argumentsList) {
        console.log('target', target) // sum()这个函数本身
        console.log('thisArg', thisArg) // undefined
        console.log('argumentsList', argumentsList) // [1, 2]

        return target(argumentsList[0], argumentsList[1]) * 10
    }
}

const proxy1 = new Proxy(sum, handler)

console.log(sum(1, 2)) // expected output: 3
console.log(proxy1(1, 2)) // expected output: 30
```

```js
var p = new Proxy(function() {}, {
    apply: function(target, thisArg, argumentsList) {
        console.log('called: ' + argumentsList.join(', ')) // called: 1, 2, 3
        
        return argumentsList[0] + argumentsList[1] + argumentsList[2]
    }
})

console.log(p(1, 2, 3)) // 6
```

### construct
含义：拦截new命令  

`target`：目标对象。  
`args`：构造函数的参数列表。  
`newTarget`：创建实例对象时，new命令作用的构造函数（下面例子的p）。

```js
var p = new Proxy(function () {}, {
    construct: function (target, args, newTarget) {
        console.log('called: ' + args.join(', '));
        return {
            value: args[0] * 10
        }; // return 1 //Uncaught TypeError: 'construct' on proxy: trap returned non-object ('1')
    }
});

(new p(1)).value
// "called: 1"
// 10

```

### this指向

proxy 会改变 target 中的 this 指向，一旦 Proxy 代理了 target，target 内部的 this 则指向了 Proxy 代理

```js
const target = new Date('2021-01-03')
const handler = {}
const proxy = new Proxy(target, handler)

console.log(proxy.getDate())
```

运行代码，会发现报错，提示`TypeError: this is not a Date object.`，即 this 不是 Date 对象的实例，这时需要我们手动绑定原始对象即可解决：

```js
const target = new Date('2021-01-03')
const handler = {
  get(target, prop) {
    if (prop === 'getDate') {
      return target.getDate.bind(target) // 绑定
    }
    return Reflect.get(target, prop)
  },
}
const proxy = new Proxy(target, handler)

console.log(proxy.getDate()) // 3

```


### Reflect
[03\_02\_理解 Proxy 和 Reflect开启掘金成长之旅！这是我参与「掘金日新计划 · 12 月更文挑战」的第4 - 掘金](https://juejin.cn/post/7171655019425431583?searchId=202408052034543D34914976ABFC864D67)

`Reflect`又叫反射，设计的目的主要有以下几个：
（1）将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。也就是说，从`Reflect`对象上可以拿到语言内部的方法。

（2）修改某些`Object`方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)` 在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

（3）让`Object`操作都变成`函数行为`。某些`Object`操作是`命令式`，比如`name in obj`和`delete obj[name]`，而`Reflect.has(obj, name)`和`Reflect.deleteProperty(obj, name)`让它们变成了函数行为，。

（4）其实可能你已经注意到了，`Reflect`对象的方法与`Proxy`对象的方法一一对应，只要是`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便地调用对应的`Reflect`方法，完成默认行为，作为修改行为的基础。`Proxy`可以捕获13种不同的基本操作，这些操作有各自不同的`Reflect API`方法。
- `Reflect.get()` → 读取属性
- `Reflect.set()` → 设置属性
- `Reflect.has()` → 属性是否存在，等同于in
- `Reflect.defineProperty()` → 定义属性
- `Reflect.getOwnPropertyDescriptor()` → 获取指定属性的描述对象
- `Reflect.deleteProperty()` → 删除属性，等同于delete
- `Reflect.ownKeys()()` → 返回自身属性的枚举
- `Reflect.getPrototypeOf()` → 用于读取对象的__proto__属性
- `Reflect.setPrototypeOf()` → 设置目标对象的原型（prototype）
- `Reflect.isExtensible()` → 表示当前对象是否可扩展
- `Reflect.preventExtensions()` → 将一个对象变为不可扩展
- `Reflect.apply()` → 调用函数，等同于等同于 Function.prototype.apply.call()，但借用原型方法可读性太差
- `Reflect.construct()` → 等同于new

