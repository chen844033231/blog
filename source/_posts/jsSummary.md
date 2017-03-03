title: JS高级程序设计第三版注意点
date: 2017-2-27 17:39:25
categories:
- WEB
- 基础
tags:
- javascript
---
![](/blog/css/images/jsSummary.jpg)

# JS高级程序设计第三版注意点

# JS简介

1. js诞生于1995年主要用来处理前端表单验证逻辑,随后发展为完整的客户端语言
2. js由js核心(ECMASCRIPT),DOM,BOM组成 (js可以脱离浏览器执行)
3. 要想全面了解js,需要了解它的本质,局限性和特点

# 在HTML中使用js

1. 在使用`<script>`标签嵌入脚步时,不要在代码中加入`</script>`,因为会被解析为脚步结束而出错
2. 带有src属性的`<script>`标签,标签之间的代码不会被执行

# 基本概念

1. undefined, null, number, string, boolean, symbol 6种基本类型, object引用类型
2. 严格模式: with禁用, 必须声明变量, 直接调用函数内部this是undefined, delete删除错误会报错, 同名参数会报错, apply,call第一个参数为null,undefined不在转换
3. apply的参数接受数组类型,然后对于解析到参数上. call接受的参数可以为任意类型
4. Number('') === 0, parseInt('', 10) === NaN, 始终给记得写第二个参数
5. ~, &, |, ^, >>, <<, >>>,操作二进制数

# 变量, 作用域和内存问题

1. 原始类型和引用类型, 在为原始类型操作方法的时候,会临时产生一个包装对象
2. 函数执行会往上层作用域查找参数
3. js具有自动垃圾回收机制,实现方式目前采用标记清除法

# 引用类型

1. `Function.prototype === Object.__proto__`
2. slice,substr,substring,在处理负数的时候有点区别
3. Global对象定义为ECMASCRIPT的全局对象,各个实现方式不同
4. 判断数组方法: Object.prototype.toString.apply(testArray)

# 面向对象程序设计
1. 数据属性: configurable enumerable writable value 修改数据属性: Object.definedPeoperty
2. 一次定义多个属性: Object.definedProperties
3. 工厂模式: 定义一个方法,调用该方法,返回对象(没有用到原型链)
4. 构造函数模式: 定义一个构造函数,采用new方法来新建对象(对象的方法会被多次创建)
5. 原型模式: 定义一个构造函数,采用new方法创建对象.对象的属性和方法挂载到原型对象上(对象无自己的私有属性)
6. 组合构造函数和原型对象: 定义一个构造函数,对象的属性写入构造函数中,对象的方法写入原型对象上
7. 动态原型对象: 定义构造函数的时候,内部判断该对象是否有这个方法
8. 寄生构造函数模式: 定义一个函数,采用new返回对象.但是内部是新建对象然后返回对象的
9. 继承如果采用子类的原型对象是父类的实例,就会继承父类的属性.(子类没有自己的属性,共用原型对象上的属性)
10. 继承如果采用子类的原型对象是父类的原型对象,子类如果定义新方法,父类也会有,而且需要改变子类的constructor
11. 寄生组合式继承.子类通过构造函数中的call来调用父类的构造函数从而让子类拥有自己的和父类相同的属性.通过新建一个父类构造函数的新对象,改变新对象的constructor来得到子类的原型对象

# 函数表达式
1. 递归函数是通过自身调用自己,记得要有跳出的条件
2. 匿名函数和闭包不是一个概念,匿名函数是指的一个没有名字的函数,闭包指的是可以访问其他函数作用域的函数(从而让垃圾回收机制在函数被调用后还保留)
3. (obj.getName)() 和 obj.getName()  一样, (obj.getName = obj.getName)() 这样是单独调用的函数
4. 私有变量和静态私有变量的定义方法

# BOM & DOM
1. 采用能力检测而不是客户端检测机制
2. documentFragment类型
3. querySelector 和 querySelectorAll 和 getElementsByClassName
4. classList 的四个方法 add, remove, toggle, contains
5. scrollIntoView方法
6. 事件冒泡和事件捕获