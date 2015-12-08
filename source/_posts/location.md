title: 导航高亮
date: 2015-12-2 10:31:05
categories:
- WEB
- 技巧
tags:
- react
- 导航
---
![](/blog/css/images/location.jpg)

## 非路由菜单高亮
传统做法是用class来实现,首先初始化一个高亮按钮,然后通过点击取消掉全部class,然后在给点击的添加class即可.

采用jq做法如下:
```js
<ul id="nav">
  <li class="item cur">1</li>
  <li class="item">2</li>
  <li class="item">3</li>
</ul>
<script>
  var items = $('#nav li')
  items.click(function() {
    var index = this.indexof(items)
    items.removeClass('cur').eq(index).addClass('cur')
  })
</script>
```
上面的做法需要自己去操作dom节点,而如今流行框架,采用数据的格式来操作dom节点,除非需要复杂的操作.否则抽象化为对象来处理.

采用对象来处理菜单高亮:
```js
<ul>
  {#each: menu,index}
  <li class={menu.activeClass} data-index={index}>{menu.itemName}</li>
  {/each}
  }
</ul>
<script>
  // es6写法
   menu = [
    {
      itemName : '1',
      activeClass: 'cur',
    },
    {
      itemName : '2',
      activeClass: '',
    },
    {
      itemName : '3',
      activeClass: '',
    },
  ]

  handleClick(e) {
    const curIndex = e.target.getAttribute('data-index')
    menu.forEach((item, index) => {
      if(curIndex === index) {
        item.activeClass = 'cur'
      }else {
        item.activeClass = ''
      }
    })
  }
</script>
```

# 路由高亮
由于点击高亮和路由高亮有一个重要的区别,就是路由高亮需要涉及到location对象.(服务端路由高亮这里不讨论)

首先介绍下location对象.这是浏览器(BOM)模型中的一员.挂载到window下.主要负责地址栏上的东西.包括路由重载,替换功能.
```js
// 可以看下location对象的属性.
// 假设url是:http://www.baidu.com:3000/app/dashboard/?name=marchen#index/name
我们采用console.log(location),输出如下结果:
{
  ancestorOrigins: DOMStringList,  // 不清楚什么作用
  assign: function(){},  // 加载一个新的文档,这是不改变url加载新文档的方法
  hash: '#index/name',
  host: 'www.baidu.com:3000',
  hostname: 'www.baidu.com',
  href: 'http://www.baidu.com:3000/app/dashboard/?name=marchen#index/name',
  origin: 'http://',
  pathname: '/app/dashboard/',
  port: 3000,
  protocol: 'http:',
  reload: function(){}, // 重载
  replace: function(){}, // 替换
  search: '?name=marchen'
}
```

其中高亮路由,也就是根据路径来激活类.需要采用判断路径的方法.如果采用react来实现的话,可以把逻辑封装到组件里面.如果不是的话.可以采用最简单方法:
```js
<ul>
  <li class={location.pathname.include(menu.href)}>{menu.name}</li>
</ul>
```