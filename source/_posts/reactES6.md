title: 用ES6来写React组件
date: 2015-11-25 17:31:54
categories:
- WEB
- 框架
tags:
- react
- es6
- babel
---
![](/blog/css/images/react.jpg)

React是facebook开源的一款前端UI库,主要特性是采用虚拟dom,和单向数据流来构建应用程序.

React不是一个完整的框架,它仅仅相当于MVC中的V,用React来构建应用程序,可以很方便的构建动态页面,因为它在内存中模拟浏览器的dom,然后diff判断需要更新的最小dom节点,而这些过程都是React内置完成的,效率相当高效,你不必手动操作dom了. 随着React的流行,一系列React的生态圈也建立起来.
* [React](http://facebook.github.io/react/index.html) React官网
* [React Native](http://facebook.github.io/react-native/) 采用web技术来开发ios和android应用
* [redux](http://redux.js.org/) 单向数据流flux架构
* [awesome-react](https://github.com/enaqx/awesome-react) 一系列和react相关的技术

React提出组件化开发应用程序的思想,其实组件化开发是很久就提出来的,不过随着nodejs和js语法不断完善和发展,今年才被正式提出来,而React库的基本开发模式也是组件化开发.虽然开始的时候会有一点点麻烦,可是随着开发的进行,会发现组件化开发是多么的明智,而多做的而外工作也是值得的.

我下面的内容会假定你已经大致了解过React,并且对组件化开发有一定的基础.否则可以去看下react的官网.下面主要是讲解react在es6中的语法,如何定义react组件和组件内部的方法.

ES6在2015年正式定稿,为js添加了很多新的特性,废弃一部分糟粕的js语法.不过现在主流客户端(浏览器)对es6的支持度不是很好,nodejs对es6的支持也不是很全部.所以想用es6的语法来写客户端的程序,就需要把es6的语法转换成es5的语法,这样浏览器就能执行了.而比较著名的编译器就是[babel](http://babeljs.io/)和[traceur](https://github.com/google/traceur-compiler).我们这里选用babel主要是支持编译react库的语法.(react官方指定编译器,你懂的)

采用es6来写react组件是一件很爽的事情,我已经忘记大部分react原生的方法了.用es6来写react组件更加的容易.

### 类
每个react的组件都是一个对象,而react专门写了一个组件类来让es6语法可以继承这个类.从而创建新的组件.这个类就是`Component`,这个组件完整路径是`React.Component`.
我们只需要继承这个类,就能创建出react组件了.
```js
import React from 'react'
class Hello extends React.Component {
	render() {
		return(
			<h1> hello boy! </h1>
		);
	}
}
```

组件内部可以写很多方法,有react内置的生命周期方法,还可以自定义一些方法.如果采用es5的方法来写:
```js
// ES5的定义组件方法
var Hello = React.createClass({
	componentDidMount: function() { ... },
	render:	function() { ... }
})
```
```js
// ES6定义组件方法
import React, { Component } from 'react'
class Hello extends Component {
	component() {
		...
	}
	render() {
		...
	}
}
```

在ES5中如果想要定义组件的初始化state,需要使用getInitialState,而ES6采用constructor来定义初始化state.
```js
// ES5方式定义初始化state
var Hello = React.createClass({
	getInitialState: function() {...}
})
```
```js
// ES6方式定义初始化state
import React, { Component } from 'react'
class Hello extends Component {
	constructor(props) {
		super(props);  // 这个一定要添加
		this.state = {  // 给state赋初始值
			greet : 'hello'
		}
		...
	}
	render() {
		...
	}
}
```

ES5中定义初始化props需要使用getDefaultProps,定义propTypes需要propTypes方法,而ES6采用静态static方法.
```js
// ES5定义props和propTypes
var Video = React.createClass({
  getDefaultProps: function() {
    return {
      autoPlay: false,
      maxLoops: 10,
    };
  },
  getInitialState: function() {
    return {
      loopsRemaining: this.props.maxLoops,
    };
  },
  propTypes: {
    autoPlay: React.PropTypes.bool.isRequired,
    maxLoops: React.PropTypes.number.isRequired,
    posterFrameSrc: React.PropTypes.string.isRequired,
    videoSrc: React.PropTypes.string.isRequired,
  },
});
```
```js
// ES6方式定义初始化props和propTypes
class Video extends React.Component {
  constructor(props) {
  	super(props)
  	this.state = {
    	loopsRemaining: this.props.maxLoops,
  	}
  }
  static defaultProps = {
    autoPlay: false,
    maxLoops: 10,
  }
  static propTypes = {
    autoPlay: React.PropTypes.bool.isRequired,
    maxLoops: React.PropTypes.number.isRequired,
    posterFrameSrc: React.PropTypes.string.isRequired,
    videoSrc: React.PropTypes.string.isRequired,
  }
}
```
上面的需要注意的是,babel6默认是不转换任何代码的,所以我们会加上babel6 preset.不过static属性是不包含在react包里面的.所以需要添加一个插件.整体的`.babelrc`文件看起来是这样:
```js
{
	presets: ['react', 'stage-0', 'es2015'],
	plugins: ["transform-class-properties"]
}
```

## 箭头函数
在ES5的定义中,因为createClass函数绑定了this到组件的实例,所以在定义组件的内部可以直接使用this来调用组件实例.而ES6由于没有绑定this到组件的实例,所以需要手动绑定this到组件的方法.
```js
// ES5方法,内部的this绑定到了组件实例上
var Hello = React.createClass({
	greet: function() {
		this.state = { ... }
	}
})
```
```js
// ES6手动绑定this
class Hello extends Component {
	contructor(props) {
		super(props)
		this.greet = this.greet.bind(this)
	}
	greet() {
		this.state = { ... }
	}
}
```

看起来这样手动绑定this关键字很麻烦,不要注意的是,组件的方法,比如componentDidMount,getInitialState这些方法是组件自己调用的所有this指的是组件实例,不需要绑定,如果不是这些方法,而是一些dom事件的回调函数的话,this的值就是dom节点,不是组件的实例了,也就不能写this.state这样的语句了.

不过好消息是可以采用箭头函数来定义组件的方法,因为箭头函数会自动绑定this(实际上babel转化成es5代码而已).就能不用手动绑定那么麻烦啦.
```js
// ES6自动绑定this
class Hello extends Component {
  // 注意这里的写法
	greet = () => {
		this.state = { ... }
	}
}
```

## 动态属性名称和模板字符串

ES6支持动态的属性名称.还支持模板字符串,以前用es5连接字符串和变量很麻烦,需要使用+号来间接,如果变量比较多了,看起来就很乱.而ES6支持的模板字符串可以很好的解决这一问题.
```js
// ES6模板字符串
class Form extends React.Component {
  onChange(inputName, e) {
    this.setState({
      [`${inputName}Value`]: e.target.value,
    });
  }
}
```

## 解构赋值和分解属性
我们可以用解构复制来得到你想要的组件属性.然后用分解属性的方法来往下面传播属性,采用这样的方式可以提取出props中的UI变量和数据.从而控制往下面传播的值是什么:
```js
// ES6
class AutoloadingPostsGrid extends React.Component {
  render() {
    var {
      className,
      ...others,  // contains all properties of this.props except for className
    } = this.props;
    return (
      <div className={className}>
        <PostsGrid {...others} />
        <button onClick={this.handleLoadMoreClick}>Load more</button>
      </div>
    );
  }
}
```
多使用ES6的新特性,可以更好的编写react组件. 不过ES6不支持mixins,幸运的是可以采用插件让es6支持mixins.不过我们可以采用class继承的方式,把mixin写出基础的组件,然后继承这个组件就可以了.

希望这篇文章能很好的帮助了解到如何用ES6语法来写React组件.