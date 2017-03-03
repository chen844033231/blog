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

5. 自定义事件(观察者模式)实现原理
```js
/**
 * 自定义事件
 */

// 扩展编码 1. 事件冒泡(采用原型链)

function Event() {
    this.eventStack = {};
    this.parent = null;
}

// 增加事件
Event.prototype.on = function (type, fn) {
    if (typeof type === 'string' && typeof fn === 'function') {
        if (!this.eventStack[type]) {
            this.eventStack[type] = [fn];
        } else {
            this.eventStack[type].push(fn);
        }
    }
    return this;
}

// 移除事件
Event.prototype.off = function (type, fn) {
    // 移除所有事件
    if (typeof type === 'undefined' && typeof fn === 'undefined') {
        this.eventStack = {};
    } else if (typeof type === 'string' && typeof fn === 'undefined') {
        delete this.eventStack[type];
    } else if (typeof type === 'string' && typeof fn === 'function') {
        if (this.eventStack[type]) {
            this.eventStack[type] = this.eventStack[type].filter(value => value !== fn);
        }
    }
    return this;
}

// 触发事件
Event.prototype.dispatch = function (type, ...argu) {
    if (this.eventStack[type]) {
        this.eventStack[type].forEach(fn => {
            fn.apply(this, argu);
        });
        let parentObj = this.parent;
        while (parentObj) {
            parentObj.dispatchOwn(type);
            parentObj = parentObj.parent;
        }
    }
    return this;
}

// 触发自己的事件
Event.prototype.dispatchOwn = function (type, ...argu) {
    if (this.eventStack[type]) {
        this.eventStack[type].forEach(fn => {
            fn.apply(this, argu);
        });
    }
    return this;
}

// 绑定父级事件
Event.prototype.bindParent = function (parentObj) {
    if (typeof parentObj === 'object') {
        this.parent = parentObj;
    }
}

// 测试
const event = new Event();
const parentEvent = new Event();
const grandEvent = new Event();

event.bindParent(parentEvent);
parentEvent.bindParent(grandEvent);

const clickEvent1 = function () {
    console.log('you click me 1')
}

parentEvent.on('click', function() {
    console.log('you click parentEvent');
});

grandEvent.on('click', function() {
    console.log('you click grandEvent');
});

event.on('click', clickEvent1)
    .on('click', function (a, b) {
        console.log(a);
        console.log(b);
        console.log('you click me 2');
        console.log(this);
    });

event.on('move', function () {
    console.log('you move me')
});

event.dispatch('click')


event.off('click', clickEvent1);

event.dispatch('move');
event.dispatch('click', 1, 2);
```