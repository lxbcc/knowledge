## Number

![[Pasted image 20231021160222.png]]
对于js,所有数字都是按64位形式存入，不分浮点型和整型，所以在js中，1=== 1.0
- 第0位表示符号位,s,正数的话是0，负数的话是1
- 第1位到第11位表示指数部分，e
- 第12位到第63位表示尾数部分，f
```js
// 整数小数判等相同
console.log(42 === 42.0) // true
```
### valueOf
获取到Number对象的值
```js
let num = new Number(123);
console.log(num)//Number{123}
console.log(num.valueOf());//123;
```
### toFixed(index)
index:小数点保留位数，四舍五入  
把数字转换为字符串，结果的小数点后有指定位数的数字。
```dart
let num = 3.1415926;
console.log(num.toFixed(3));//3.142
```
### toString()
可把一个 Number 对象转换为一个字符串，并返回结果
### toPrecision()
把数字格式化为指定的长度:
```js
var num = new Number(13.3714);
var n = num.toPrecision(2);
// 13
```
### MAX_VALUE
获取JavaScript中最大值
```jsx
let max = Number.MAX_VALUE;
console.log(max);//1.7976931348623157e+308
```
### MIN_VALUE
获取JavaScript中最小值

```jsx
let min = Number.MIN_VALUE;
console.log(min);//5e-324
```

### Number.parseFloat
**Number.parseFloat()**
解析一个参数并返回一个浮点数。如果无法从参数中解析出数字，则返回NaN

### Number.parseInt()
方法解析字符串参数并返回指定基数或基数的整数。
### Math
#### random
语法: Math.random()
作用: 得到一个0~1之间的随机数, 包含0,不包含1
#### round
语法: Math.round()
作用: 将数据 四舍五入取整
#### ceil
* 语法: Math.ceil()
* 作用: 将数据 向上取整
#### floor
* 语法: Matn.floor()
* 作用: 将数据 向下取整
#### abs
* 语法: Math.abs()
* 作用: 取数据的绝对值
#### sqrt
* 语法: Math.sqrt()
* 作用: 求平方根
#### pow
* 语法: Math.pow(基数, 幂)
* 作用: 求一个基数的 X 次幂
#### max
* 语法: Math.max(数据1, 数据2, 数据3, ...)
* 作用: 求参数中 的 最大值
#### min
* 语法: Math.min(数据1, 数据2, 数据3, ...)
* 作用: 求参数中 的 最小值
#### PI
* 语法: Math.PI
* 作用: 求圆周率

## String

**concat** 返回字符串值，该值包含了两个或更多个提供的字符串的连接。 str.concat(str1,str2);

**indexOf**  返回 String 对象内第一次出现子字符串的字符位置。  str.indexOf('test');

**lastIndexOf**  返回 String 对象中子字符串最后出现的位置。  str.lastIndexOf('test');

**slice** 返回字符串的片段。 str.slice(start,end);
在该索引处结束提取原数组元素（从0开始）。 slice 会提取原数组中索引从  begin 到  end 的所有元素（包含begin，但不包含end）。
slice(1,4) 提取原数组中的第二个元素开始直到第四个元素的所有元素 （索引为 1, 2, 3的元素）。
如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 slice(-2,-1) 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。

**split** 将一个字符串分割为子字符串，然后将结果作为字符串数组返回。 str.split('==');

**substr**  返回一个从指定位置开始的指定长度的子字符串。str.substr(start [, length ])
```
str.substr(5，12)；从5开始长度12
```
如果 length 为 0 或负数，将返回一个空字符串。  
如果没有指定 length，则子字符串将延续到 stringObject 的最后。  
如果 start 或 length 为负数，那么它将被替换为 0。

**substring**  返回位于 String 对象中指定位置的子字符串。 str.substring(start, end);
	start（必需）：一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。参数说明：
	stop（可选）：一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多 1。
	如果 start 与 end 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。  
	如果 start 比 end 大，那么该方法在提取子串之前会先交换这两个参数。  
	如果 start 或 end 为负数，那么它将被替换为 0。

**toLowerCase**  返回一个字符串，该字符串中的字母被转换为小写字母。  str.toLowerCase( )

**toUpperCase**  返回一个字符串，该字符串中的所有字母都被转化为大写字母。 str.toUpperCase( )

**toString**   返回对象的字符串表示。 obj.toString([radix])
    radix 可选项。指定将数字值转换为字符串时的进制。