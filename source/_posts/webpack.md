title: webpack怎么用
date: 2015-12-8 14:45:25
categories:
- WEB
- 框架
tags:
- webpack
- 部署
---
![](/blog/css/images/webpack.jpg)

## 前言
最近几天在做关于webpack的项目.在使用过程中,遇到了遇到问题,不过一一解决了,我打算把这篇文章写出webpack的手册.等到自己下次还需要使用webpack的时候,能及时的参看这里的终结.

## 什么是webpack
webpack是一个打包工具,可以把文件合并在一起.不过相比于其他的打包工具,webpack可以处理静态资源,比如图片,音频,`css`,`less`,`sass`这样的文件.在webpack的世界里面,所有的都是可以被引用的.这样的好处是什么呢?我觉得主要有下面的几个好处:
1. 模块化开发(这里主要指js文件模块化开发,当然`css`可以模块化,参看[css-modules](https://github.com/css-modules/css-modules)).
2. 打包不只是单单合并文件,还可以用loader来处理文件,也就是能把`ES6`编译成`ES5`,把`less`,`sass`之类的编译成`css`.而且`css`还可以通过`autoprefixer`来处理兼容性问题.
3. 在处理静态文件,比如图片,html文件的时候,可以采用`url-loader`和`file-loader`来让图片进行`hash`处理,比较有用的一个是,还可以根据图片的大小来把图片进行base64处理.
4. code spilting可以在需要代码的时候动态加载.
5. 在发布的时候,还可以用插件来压缩js,压缩css等处理.
6. 开发的时候,可以用热替换模块,让开发根据方便.

## 如何使用
首先,webpack是用javascript写的,可以在nodejs上运行,所以要使用webpack,需要安装[nodejs](https://nodejs.org/en/),在安装完后,安装webpack:
```shell
$ npm install webpack -g
```
注意: 在windows系统上全局安装可能需要你用管理员身份打开命令行工具,其他平台可能需要你授予权限.


上面的是全局安装webpack,安装后,在命令行输入:
```shell
$ webpack
webpack 1.12.9
Usage: https://webpack.github.io/docs/cli.html

Options:
  --help, -h, -?
  ...
```
出现上面的,就代表你安装成功了.

不过推荐采用本地安装模式,这样可以防止版本冲突.本地安装方式:
```shell
$ npm install webpack --save-dev
```
安装成功后,我们新建立2个文件就叫index.js和hello.js
```js
// index.js
const hello = require('./hello.js')
console.log(hello)
```
```js
// hello.js
module.exports = 'hello world'
```
我们可以看到,我们采用了commonjs的写法,这样的代码本来只能在像nodejs这样的解析器上运行,要让他在浏览器端运行,就需要通过打包工具来处理,比如webpack.

我们采用nodejs的方式来调用webpack.新建一个文件,webpack.js最简单的配置方式:
```js
// webpack.js
const webpack = require('webpack')  //需要node版本5.0.0

webpack({
  entry: './index.js',
  output: {
    path: './',
    filename: 'bundle.js'
  }
}, function(err){
  if(err) {
    throw err
  }
  console.log('bundle successed!')
})
```

我们在运行webpack.js文件.
```shell
$ node webpack.js
```
会发现同级目录下多出了bundle.js文件.这个就是我们打包成功的文件.可以运行bundle.js这个文件,看看是不是输出'hello world'字样.

来点复杂一点的配置,让webpack可以监视文件改变,自动打包:
```js
// webpack
const webpack = require('webpack')

const bundler = webpack({
  entry: './index.js',
  output: {
    path: './',
    filename: 'bundle.js',
  },
  profile: true,
})

bundler.watch({
    aggregateTimeout: 300,  // 文件改变延迟编译时间
    poll: false  // 文件改变重新编译,设置成true或者数字会根据时间间隔轮询文件变化.单位ms.true是系统自动决定间隔时间
}, function(){
  console.log('bundle successed!')
})
```

## loader的使用
webpack原生支持commonjs.ADM,ES6等模块化语法,如果我们想让webpack处理比如`less`,`ES6`这样的语法的时候,就需要用到loader,所谓的loader(本质是一个函数)就是webpack会在处理文件的时候,调用这个函数.就能编译`less`,`sass`,`ES6`这样的语法了.推荐几个常用的loader:
### babel-loader
能把ES6语法编译成ES5语法,最新的babel6需要在.bablerc配置下需要编译什么样的语法.

### url-loader file-loader
url-loader可以把小图片变成base64位的字符,file-loader可以对文件名称进行hash等处理.便于缓存.

### post-css
可以编译css文件,结合各种css插件,可以增强css语法.

### autoprefixer
可以处理css兼容问题,比如css文件中写boder-radisu:5px;autoprefixer会根据can i use的兼容语法来添加前缀.

### script-loader
可以全局运行一个.js文件.比如有的js文件不是模块化编写的,可以采用这样的方式.

## html-loader
可以把html文件变成字符串.

loader的语法:
```js
webpack({
  module: {
  loaders : [
    {
      test: /\.js$/,
      loader: 'babel-loader',
    }
  ]
  }
})
```

## plugins的使用
webpack有许多的插件,大多是webpack自带的插件.可以压缩js文件,提取相同的模块,热替换等等.

plugins的语法:
```js
webpack({
  plugins: [
    new webpack.optimize.OccurenceOrderPlugin(),
  ]
})
```

## 缓存的处理
每个文件都需要用require()包装起来.

在前端部署的过程中,需要考虑到文件版本的问题,所以采用每个文件的名字hash化,这样可以缓存起来.在使用中需要配置[hash]或者[chunkhash]来处理.在处理完hash之后,如果你只有一个进入点,那么html文件需要通过`assets-webpack-plugin`来得到hash的进入点. 然后替换掉html中的js文件路径.可以采用gulp来替换.采用`gulp-html-replace`来替换html路径.采用`fs-extra`来建立文件夹目录和复制文件.

## 发布处理
采用git分支处理发布. 源文件分支master发布后可以把文件传到production分支.(还没有想到方案)

## 错误处理
最好的方式是用插件把每个模块包装起来,然后出错后可以报出事那个模块出文件了.

## 打包慢处理方式
确保没有打包额外的代码,可以用include来引入需要编译的包.去除node_modules目录等操作. 引入需要引入的包.

## 动态化加载需要的模块
可以用:
```js
require.ensure([], function(require){
  require('a.js')
})
```

## 后缀自动查找
使用resolve.extensions来设置自动补全后缀的值,注意`第一个值是空字符串!`,虽然可以自动查找后缀,不过还是建议加上后缀
```js
{
  reslove: {
    extensions: ['', '.css', '.js']
  }
}
```

## 打成多个包
如果一个进入点只打成一个包的话,webpack可以有多个进入点,打包成多个包.配置如下:
```js
{
  entry: {
    page1: './page1/index.js',
    page2: './page2/index.js',
  },
  output: {
    path: path.join(__dirname,'./build'),
    filename: '[name].js',
  }
}
```

## cdn
由于把文件做了version处理,并且首页的bundle.js需要做cnd处理的时候,可以改变output.publicpath.不过在做html文件替换的时候,还是可以需要替换cnd路径的.

## 单独提取css文件
ExtractTextPlugin插件可以单独把css文件提取出来.因为在用css-loader的时候,发现大的文件显示不出来.提取出来就可以的.(暂时没有找到解决方案).

## 开发服务器
可以采用borowerSycn结合webpack-hot-middleware和webpack-dev-middleware来开发.热替换,流刷新,同步操作都有了.

目前这样一套开发流程我已经开源在github,欢迎star:[react-workflow](https://github.com/chen844033231/react-workflow)