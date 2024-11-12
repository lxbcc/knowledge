[map方法会不会改变原数组？\_map会改变原数组吗\_饱饱\~\~的博客-CSDN博客](https://blog.csdn.net/m0_58893670/article/details/124370889)

### JavaScript Date对象
[JavaScript Date 对象 | 菜鸟教程](https://www.runoob.com/jsref/jsref-obj-date.html)
Date 对象用于处理日期与时间。
创建 Date 对象： new Date()

```js
var d = new Date(); // 2024-06-20T07:36:06.317Z
var d = new Date(milliseconds); // 参数为毫秒
var d = new Date(dateString);
var d = new Date(year, month, day, hours, minutes, seconds, milliseconds);
```

<mark class="hltr-cyan">milliseconds</mark> 参数是一个 Unix 时间戳（Unix Time Stamp），它是一个整数值，表示自 1970 年 1 月 1 日 00:00:00 UTC（the Unix epoch）以来的毫秒数。

<mark class="hltr-cyan">dateString</mark> 参数表示日期的字符串值。

<mark class="hltr-cyan">year, month, day, hours, minutes, seconds, milliseconds</mark> 分别表示年、月、日、时、分、秒、毫秒。

方法

getDate()： 从 Date 对象返回一个月中的某一天 (1 ~ 31)。

getDay() : 从 Date 对象返回一周中的某一天 (0 ~ 6)。

getFullYear() : 从 Date 对象以四位数字返回年份。

setHours()：设置 Date 对象中的小时 (0 ~ 23)。


```js
var d = new Date()

// 获取年月日
var y = d.getFullYear();
// ['一月'， '二月'] 注意：月份要 + 1
var m = d.getMonth() + 1
var r = d.getDate()
console.log(y + '年' + m + '月' + r + '日');


// 时分秒
var s = d.getHours()
var f = d.getMinutes()
var x = d.getSeconds() 
console.log(s + '时' + f + '分' + x + '秒');

// 获取星期
var week = ['星期天', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六']
var w = d.getDay()
console.log(week[w]);

// 补零函数
function zero(n) {
    return n >= 10 ? n : '0' + n
}
```


时间戳

```js
// Date.now()
// Date.now()可以获得当前的时间戳
console.log(Date.now()) //1642471441587

// Date.parse()
// Date.parse()将字符串或者时间对象直接转化成时间戳
Date.parse(new Date()) //1642471535000
Date.parse("2022/1/18 10:05") //1642471500000

// valueOf
// 通过valueOf()函数返回指定对象的原始值获得准确的时间戳值
(new Date()).valueOf() //1642471624512

// getTime
// 通过原型方法直接获得当前时间的毫秒值，准确
new Date().getTime() //1642471711588

// Number
// 将时间对象转化为一个number类型的数值，即时间戳
Number(new Date()) //1642471746435
```


### groupBy

Object.groupBy() 是一种静态方法，按属性对数组数据进行分组。只需传入两个参数：数组和回调函数。对数组中的每个元素执行回调函数以确定其所属的组。

- Object.groupBy
```js
Object.groupBy(items, callbackFn) // 语法，第一个参数是可迭代对象，第二参数是回调函数
```

- Map.groupBy
```js
Map.groupBy(items, callbackFn) // 语法，第一个参数是可迭代对象，第二参数是回调函数
```


```js
const data = [
  { id: 1, type: 'A' },
  { id: 2, type: 'B' },
  { id: 3, type: 'A' },
  { id: 4, type: 'C' },
  { id: 5, type: 'B' },
];
const groupType = Object.groupBy(data, 'type') // ❌ 第二个参数必须是函数
const groupType = Object.groupBy(data, (item) => item.type) // ✅ 按照类型进行分类， 并且 type 就是 group 的 key 值。
// {
//   A: [{…}, {…}]
//   B: [{…}, {…}]
//   C: [{…}]
/ }

```

实现一个按照传递属性方式实现一个 groupBy：

```js
function groupBy(arr, key) {
  return arr.reduce((acc, obj) => {
    const groupKey = obj[key];
    acc[groupKey] = acc[groupKey] || [];
    acc[groupKey].push(obj);
    return acc;
  }, {});
}

// data 复用以上的 data
groupBy(data, 'type') // 输出结果与 Object.groupBy 一致
```

通用的 groupBy

```js
type KeyFunc<T> = (obj: T) => string

export function groupByWithFn<T>(
  array: T[],
  keyFunc: KeyFunc<T>
): Record<string, T[]> {
  return array.reduce((acc: Record<string, T[]>, obj: T) => {
    const key: string = keyFunc(obj)
    if (!acc[key]) {
      acc[key] = []
    }
    acc[key].push(obj)
    return acc
  }, {})
}
```