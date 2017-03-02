title: Javascript Notes
date: 2017-3-12 10:24:55
categories:
- WEB
- 技巧
tags:
- javascript
---
![](/blog/css/images/tips.jpg)

这里主要记录个人忽略的知识点:

1. node.js的require是引用传值的,也就是说,改变引用模块的值,也会改变引用的值, 多次引用同一个模块,只会拿到模块中的值, 不会重新运行模块
```js
// a.js
const b = require('./b')

// { name: 'b' }
console.log(b);

b.time = 2;

setTimeout(() => {
  // { name: 'change b', time: 2 }
  console.log(b);
}, 1000);

// b.js
const people = {
  name: 'b'
}

module.exports = people

setTimeout(() => {
  people.name = 'change b';
}, 100);
```

2. `exports` 只是 `module.exports` 的一个引用, require拿到的是module.exports的值
```js
exports = module.exports

module.exports.name = 'time'
// 等价于
exports.name = 'time'

// 不会改变module.exports的值,也就是module.exports还是{}
exports = 'time'
```

3. require的实现原理
```js
function require(...) {
  var module = { { exports: {} } };

  (function(module, exports) {
    // your module code execute here
  })(module, module.exports);

  return module.exports;
}
```

4. js执行分为2中任务队列,setTimeout,render等task队列和promise等trick队列,会先执行trick队列,后执行task队列
```js
setTimeout(() => {
  console.log(1)
}, 0)

Promise.resolve().then(() => {
  console.log(2)
});

// 运行结果
// 2
// 1
```