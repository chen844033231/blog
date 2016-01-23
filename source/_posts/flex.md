title: 使用flex来布局界面
date: 2016-3-4 14:58:22
categories:
- WEB
- 技巧
tags:
- flex
---
![](/blog/css/images/flex/flex.jpg)

## 布局的发展
以前布局采用table来布局网页,但是由于table布局需要加载完table的全部内容才能显示内容,同时由于table布局不是很灵活,所以人们发明了div+css来布局页面,这种布局的比table更加灵活,同时布局出来的页面也更好的好看,传统的div+css布局采用浮动来布局页面,这种方式沿用了很长的时间,随着响应式布局的流行,和页面布局的多样化,采用浮动布局慢慢浮现出问题,比如需要做许多的工作才能实现常见的效果,并且每次设置浮动后都需要清除浮动,这让布局这一简单但是无比重要的任务变得复杂.所以人们提出新的布局方式.flex布局.这种布局方式可以有更好的方式来控制子元素的位置.而且更加的灵活和强大.采用flex布局可以完成以往用css很难完成的布局或者不可能完成的布局.下面一起来看看flex布局的要点.

## 概念
flex布局的模型就是容器有主轴和辅轴,默认的主轴沿着x方向,辅轴沿着y方向.所以子元素按照主轴方向放置.可以通过一些css属性来控制轴线上元素的位置.
![](/blog/css/images/flex/model.png)

## 技巧
在flex布局之前,可以想象着容器如果被定义为flex的话,他的直接子元素不管是块级元素还是内联元素,都是按照主轴线放置的,块级元素并不会换行.当元素放置完成,即可通过属性来控制容器内的布局方式.我们可以把容器内的直接子元素叫做项目.

## 简单的布局实例
###### 提示: 由于笔者写文章的时候没有注意到分辨率过大,截的图片比较长,可以点击图片放大查看效果哦

最简单的布局是,1行4列,每一列宽度都相等.
![](/blog/css/images/flex/1.png)
```html
<div class="flex-container">
    <div class="flex-item">item</div>
    <div class="flex-item">item</div>
    <div class="flex-item">item</div>
    <div class="flex-item">item</div>
</div>
```
```css
.flex-container {
  display: flex;

}
.flex-item {
  flex: 1;
  margin: 10px;
  background-color: red;
}
```

## 容器可用的属性
容器可以使用的属性包括:
* flex-direction: row | column | row-reverse | column-reverse;
* flex-wrap: nowrap | wrap | wrap-reverse;
* flex-flow: [flex-direction] [flex-wrap];
* justify-content: flex-start | flex-end | center | space-between | space-around;
* align-item: flex-start | flex-end | center | stretch;
* align-content: flex-start | flex-end | center | space-between | space-around;

### flex-direction属性
flex-direction属性用来定义主轴的方向,默认的主轴方向是从左到右.元素也是按照从左到右的方式排列的.
![](/blog/css/images/flex/2.png)
```css
.flex-container {
  display: flex;
  flex-direction: row;
}
```
row值是水平排列,然后依次往后.

---
![](/blog/css/images/flex/3.png)
```css
.flex-container {
  display: flex;
  flex-direction: column;
}
```
column值是垂直排列,依次往后.

---
![](/blog/css/images/flex/4.png)
```css
.flex-container {
  display: flex;
  flex-direction: row-reverse;
}
```
row-reverse值是水平排列,但是从右往左依次排列.

---
![](/blog/css/images/flex/5.png)
```css
.flex-container {
  display: flex;
  flex-direction: column-reverse;
}
```
column-reverse值是垂直排列,但是从下到上依次排列.

### flex-wrap属性
flex-wrap是用来定义容器内元素是否换行.如果容器内元素超过容器边界,是否需要把元素换到下一行.默认是不换行.

![](/blog/css/images/flex/6.png)
```css
.flex-container {
  display: flex;
  flex-wrap: nowrap;
}
```
nowrap值是默认的值,也就是超过边界不会换到下一行.从上面的效果还可以看到,我定义的宽度默认都缩小了.如果容器不换行.容器内元素超过边界会等比缩小.

---
![](/blog/css/images/flex/7.png)
```css
.flex-container {
  display: flex;
  flex-wrap: wrap;
}
```
wrap值是让元素换行,这个效果可以产生多行.align-content属性也只对换行的容器才起效果.

---
![](/blog/css/images/flex/8.png)
```css
.flex-container {
  display: flex;
  flex-wrap: wrap-reverse;
}
```
wrap-reverse值是换行会从下往上叠加.而wrap是从上往下叠加的.

### flex-flow属性
flex-flow属性是flex-direction和flex-wrap的简写.通常我们采用flex-flow来定义上面2个属性.

![](/blog/css/images/flex/9.png)
```css
.flex-container {
  display: flex;
  flex-flow: row wrap;
}
```

### justify-content属性
justify-content属性用来定义元素的位置.可以理解为沿着主轴的对齐方式.

![](/blog/css/images/flex/10.png)
```css
.flex-container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-start;
}
```
flex-start值沿着主轴开始的地方放置.

---
![](/blog/css/images/flex/11.png)
```css
.flex-container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: flex-end;
}
```
flex-end值沿着主轴结束的地方放置.

---
![](/blog/css/images/flex/12.png)
```css
.flex-container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
```
center值把内容放置到主轴中心.

---
![](/blog/css/images/flex/13.png)
```css
.flex-container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
}
```
space-between值是两端对齐.两端的元素不留空白.

---
![](/blog/css/images/flex/14.png)
```css
.flex-container {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
}
```
space-around值会将两端留有空白,然后平均分布.

### align-item属性
align-item属性定义元素辅轴对齐方式.

![](/blog/css/images/flex/15.png)
```css
.flex-container {
  display: flex;
  align-items: flex-start;
}
```
flex-start从辅轴开始处对齐元素.

---
![](/blog/css/images/flex/16.png)
```css
.flex-container {
  display: flex;
  align-items: flex-end;
}
```
flex-end从辅轴结束处对齐元素.

---
![](/blog/css/images/flex/17.png)
```css
.flex-container {
  display: flex;
  align-items: center;
}
```
center从辅轴中心处对齐元素.

---
![](/blog/css/images/flex/18.png)
```css
.flex-container {
  display: flex;
  height: 100px;
  align-items: stretch;
}
```
stretch拉伸元素和容器一样高.

### align-content属性
align-content属性定义多行内容对齐方式,这个属性只有当容器多行时才起作用,也就是必须设置flex-wrap为wrap或者wrap-reverse才有效.

![](/blog/css/images/flex/19.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: flex-start;
}
```
flex-start值让多行内容对齐到辅轴开始处.

---
![](/blog/css/images/flex/20.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: flex-end;
}
```
flex-end值让多行内容对齐到辅轴结束处.

---
![](/blog/css/images/flex/21.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: center;
}
```
center值让多行内容沿着辅轴居中.

---
![](/blog/css/images/flex/22.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: space-between;
}
```
space-between值让多行内容两端对齐.

---
![](/blog/css/images/flex/23.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  flex-wrap: wrap;
  align-items: flex-start;
  align-content: space-around;
}
```
space-around值让多行元素两端对齐,但是两端会有空白.

## 项目元素可用的属性
* order: integer;
* flex-grow: interget;
* flex-shrink: interget;
* flex-basis: length;
* flex: [flex-grow] [flow-shrink] [flex-basis];
* align-self: flex-start | flex-end | center | stretch;

### order属性
order属性定义项目的排列顺序.数值越小,排列越靠前,默认为0,可以为负数.

![](/blog/css/images/flex/24.png)
```css
.flex-item:nth-child(1) {
  order: 1;
}
```
### flex-grow属性
flex-grow属性定义项目的放大比例,默认为0,即如果存在剩余空间,也不放大.

![](/blog/css/images/flex/25.png)
```css
.flex-item:nth-child(1) {
  flex-grow: 1;
}
```
如果有多个定义flex-grow的项目,那么剩余空间会按照权重比例分配.

### flex-shrink属性
flex-shrink属性定义了项目的缩小比例,默认为1,即如果空间不足,该项目将缩小.

![](/blog/css/images/flex/26.png)
```css
.flex-item {
  width:2000px;
  flex-shrink: 1;
}
```
如果多个项目定义此属性,则会按照权重比例分配缩小大小.

### flex-basis属性
flex-basis属性定义了在分配多余空间之前,项目占据的主轴空间（main size).浏览器根据这个属性,计算主轴是否有多余空间.它的默认值为auto,即项目的本来大小.
如果设置了长度,就会按照长度来分配项目的占据空间.

### flex属性
flex属性是上面3个属性的缩写.
flex: [flex-grow] [flex-shrink] [flex-basis];
我们才采用的写法是:
```css
.item {
  flex: 1;  /* 省略后面的2个数值 */
}
```

### align-self属性
align-self属性允许单个项目有与其他项目不一样的对齐方式,可覆盖align-items属性.默认值为auto,表示继承父元素的align-items属性,如果没有父元素,则等同于stretch.

![](/blog/css/images/flex/27.png)
```css
.flex-container {
  display: flex;
  height: 200px;
  align-items: flex-start;
  background-color: green;
}
.flex-item {
  width: 200px;
  height: 50px;
  margin: 10px;
  background-color: red;
}
.flex-item:nth-child(1) {
  align-self: flex-end;
}
.flex-item:nth-child(2) {
  align-self: center;
}
.flex-item:nth-child(3) {
  align-self: stretch;
}
```
可以看到通过设置项目的align-self可以脱离容器定义的辅轴对齐方式,十分灵活.

## 总结
flex布局十分灵活,对于以前用css布局十分难的案例,采用flex布局可以分分钟搞定.但是flex的兼容性比较差,不过目前现代浏览器都支持flex布局了.随着flex的普及和浏览器的发展,flex必定成为移动端的布局好方式.

推荐一个项目,用flex可以实现的布局案例. [https://github.com/philipwalton/solved-by-flexbox](https://github.com/philipwalton/solved-by-flexbox)