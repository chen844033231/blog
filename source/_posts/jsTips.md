title: Javascript Tips
date: 2016-1-19 10:24:55
categories:
- WEB
- 技巧
tags:
- javascript
---
![](/blog/css/images/tips.jpg)

Javascript是门灵活的语言,有许多小的技巧可能让你的JS技术更上一层,来看看下面总结的JS编码技巧你是否都知道呢?


## 总览
* [#01 变量转换成整数](#01)
* [#02 向数组中插入元素](#02)
* [#03 正确的清空对象](#03)

***
<h2 id="01">#01 变量转换成整数</h2>
在javascript中,把变量转换成整数是十分常见的操作,js原生提供的方法`parseInt`可以把字符串转换成整数.但是还有一个常见的方法是用`~~`操作符来将数字或者字符串转换成整数.例如:
```js
var str = '3.14'
var nStr = ~~str  // nstr = 3
console.log(typeof nStr)  // number
```
`~`是位操作符,按位取反.如果操作对象是字符串类型,首先把字符串类型转换成数字类型,由于`~`是对整数进行操作,所以把数字转换成整数后取反,然后再次取反,就相当于把字符串转换整数了. 

当然,这种方法对于数字也是有效的,这里相当于是把数字小数点后面的去掉取整的,和`Math.floor`方法效果相同.
```js
var count = ~~3.14   // count = 3
var price = ~~-6.24  // price = -6
```

### bonus
`~`按位取反符还可以用来模拟`includes`方法,在ES6中,字符串有一个`includes`方法来确定字符串中是否包括某个子字符串.一般我们为了达到这样的效果,会编写下面的代码:
```js
var str = 'hello world'
if(str.indexOf('world') >= 0) {
  // do something
}
/* 或者 */
if(str.indexOf('world') === -1) {
  // do something
}
```

我们可以采用下面的方法来达到相同的效果,用`~`操作符:
```js
var str = 'hello world'
if(~str.indexOf('world')) {
  // do something
}
/* 或者 */
if(!~strindexOf('world')) {
  // do something
}
```
`~`操作符会按位取反,而-1在计算机中按位取反后是0,所以如果不包含子字符串,if语句将不会执行.同理,前面加上`!`就能表达不包含子字符串的效果.


***
<h2 id="02">#02 向数组中插入元素</h2>
在js中,向数组中插入元素,一般想到的就是
`Array.prototype.push`     末尾追加元素
`Array.prototype.pop`      末尾移除元素
`Array.prototype.unshift`  头部增加元素
`Array.prototype.shift`    头部移除元素
`Array.prototype.splice`   中间增加或移除元素

除了这些之外,还有一些方法可以来增加数据的元素.
##### 向数组尾部增加元素
```js
var arr = [1,2,3,4,5]

arr.push(6)
arr[arr.length] = 6 // 43% faster in Chrome 47.0.2526.106 on Mac OS X 10.11.1
```
##### 向数组头部增加元素
```js
var arr = [1,2,3,4,5]

arr.unshift(0)
[0].concat(arr) // 98% faster in Chrome 47.0.2526.106 on Mac OS X 10.11.1
```
##### 向数组中间增加元素
```js
var items = ['one', 'two', 'three', 'four']
items.splice(items.length / 2, 0, 'hello')
```
巧妙的使用这些技巧,能更好的提升你程序的性能.


***
<h2 id="03">#03 正确的清空对象</h2>
我们在清空数组的时候,通常采用的方法如下:
```js
var arr = ['h', 'e', 'l', 'l', 'o']

// 清空数组
arr = []

console.log(arr)  // []
```
这样是可行的,不过还有另外一种更加高效,并且可靠的方式来清空数组:
```js
var arr = ['h', 'e', 'l', 'l', 'o']

arr.length = 0  // 清空数组

console.log(arr) // []
```

采用第一种赋值新的空对象的话,原来的对象还是在内存中,如果有其他变量引用了这个数组,引用的变量并不会清空.值还是原来的数组:
```js
var a = [1, 2, 3]
var b = a

a = []

console.log(a)  // []
console.log(b)  // [1, 2, 3]
/* length 方法清空 */
var a = [1, 2, 3]
var b = a

a.length = 0

console.log(a)  // []
console.log(b)  // []

```
所以,如果想清空被引用的数组的话,还是需要把原来的对象改成空对象的.同理,如果想清空对象的话:
```js
var obj = {
  greet : 'hello',
}

obj = {}  // 这样清空对象最简单,但是被引用的对象不会被清空

// 循环遍历删除对象键值
for(key in obj) {
  delete obj[key]
}
```