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
<h2 id="#02">#02 向数组中插入元素</h2>
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


