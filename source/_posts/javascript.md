---
title: javascript
date: 2022-09-02 09:01:52
tags:
- JS
categories: 方法
cover: https://w.wallhaven.cc/full/y4/wallhaven-y47kyn.jpg
---
#### let && var && const
* let声明的变量只在他所在的代码块内有效,只要块级块级作用域内存在let命令,它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。
* let不允许在相同作用域内，重复声明同一个变量。
* var命令会发生变量提升现象,即变量可以在声明之前使用,值为undefined
* const声明一个只读的常量。一旦声明，常量的值就不能改变。不可重复声明
 ```javascript
 //第一种场景
 if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
//第二种场景(内部tmp覆盖了外部的tmp)
  var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
//第三种场景(i 泄露为全局变量)
  var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
 ```
 #### ES6 新增字符串方法
* includes()：返回布尔值，表示是否找到了参数字符串。
* startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。
* endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。
```javascript
<!--这三个方法都支持第二个参数，表示开始搜索的位置。-->
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```
* repeat()
```javascript
<!--repeat方法返回一个新字符串，表示将原字符串重复n次-->
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```
* padStart()用于头部补全，padEnd()用于尾部补全。
```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```
* trimStart()消除字符串头部的空格，trimEnd()消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。
```javascript
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```
* 字符串的实例方法replace()只能替换第一个匹配。
```javascript
'aabbcc'.replace('b', '_')
// 'aa_bcc'
<!--上面例子中，replace()只将第一个b替换成了下划线。

如果要替换所有的匹配，不得不使用正则表达式的g修饰符。-->
'aabbcc'.replace(/b/g, '_')
// 'aa__cc'
<!--ES2021 引入了replaceAll()方法，可以一次性替换所有匹配。-->
'aabbcc'.replaceAll('b', '_')
// 'aa__cc'
``` 
#### 数值的扩展
* ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。
```javascript
// ES5的写法
parseInt('12.34') // 12
parseFloat('123.45#') // 123.45

// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
```
* Number.isInteger()用来判断一个数值是否为整数。
```javascript
Number.isInteger(25) // true
Number.isInteger(25.1) // false
```
#### Math对象的扩展
* Math.trunc方法用于去除一个数的小数部分，返回整数部分。
```javascript
Math.trunc(4.1) // 4
Math.trunc(4.9) // 4
Math.trunc(-4.1) // -4
Math.trunc(-4.9) // -4
Math.trunc(-0.1234) // -0
```
* Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。
```javascript
Math.sign('')  // 0
Math.sign(true)  // +1
Math.sign(false)  // 0
Math.sign(null)  // 0
Math.sign('9')  // +1
Math.sign('foo')  // NaN
Math.sign()  // NaN
Math.sign(undefined)  // NaN
```
* Math.cbrt()方法用于计算一个数的立方根。
```javascript
Math.cbrt('8') // 2
Math.cbrt(-1) // -1
Math.cbrt(0)  // 0
Math.cbrt(1)  // 1
Math.cbrt(2)  // 1.2599210498948732
```
#### 数组扩展运算
* 下面代码中,a2并不是a1的克隆,而是指向同一份数据的另一个指针,修改a2会直接导致a1的变化
```javascript
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]
```
* Array.from()用于将两类对象转换为真正的数组:类似数组的对象和可遍历的对象
```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5 的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6 的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
``` 
* Array.of()用于将一组值转换为数组
```javascript
//Array.of()基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一。
Array.of(3, 11, 8) // [3,11,8]
Array.of(3) // [3]
Array.of(3).length // 1
```
* copyWithin()在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，会修改当前数组。
* 它接受三个参数。
target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。
```javascript
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
 
```
* find()方法，用于找出第一个符合条件的数组成员。
```javascript
[1, 4, -5, 10].find((n) => n < 0)
// -5
```
* findIndex()方法的用法与find()方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。
```javascript
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```
* fill方法使用给定值，填充一个数组。
```javascript
['a', 'b', 'c'].fill(7)
// [7, 7, 7]

new Array(3).fill(7)
// [7, 7, 7]
```
* includes()方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似
```javascript
[1, 2, 3].includes(2)     // true
[1, 2, 3].includes(4)     // false
[1, 2, NaN].includes(NaN) // true
```
 * flat()用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
```javascript
 [1, 2, [3, 4]].flat()
// [1, 2, 3, 4]
[1, 2, [3, [4, 5]]].flat(2)
// [1, 2, 3, 4, 5]
[1, [2, [3]]].flat(Infinity)
// [1, 2, 3]
```
* flatMap()方法对原数组的每个成员执行一个函数（相当于执行Array.prototype.map()），然后对返回值组成的数组执行flat()方法。该方法返回一个新数组，不改变原数组。
```javascript
// 相当于 [[2, 4], [3, 6], [4, 8]].flat()
[2, 3, 4].flatMap((x) => [x, x * 2])
// [2, 4, 3, 6, 4, 8]
```
#### js基本数据类型
```javascript
  let a = [1, 2, 3, 4, 5];
        let b = 1;
        let c = "weaface";
        let d = false;
        let f = null;
        let e = new Date();
        let g = undefined;
        let h = function () {};
 
        console.log(typeof(a));//object
        console.log(typeof(b));//number
        console.log(typeof(c));//string
        console.log(typeof(d));//boolean
        console.log(typeof(e));//object
        console.log(typeof(f));//object
        console.log(typeof(g));//undefined
        console.log(typeof(h));//function
        console.log(typeof(i));//undefined

```
<font color="red">typeof()一般用于判断基本类型null除外,typeof()也可以判断function,但判断Array,null,Object</font>

