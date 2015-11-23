title: CPU踩点图
date: 2015-11-23 22:59:33
categories:
- WEB
- 特效
tags:
- canvas
- chartJS
---
![](/blog/css/images/cpu.jpg)

CPU占比探测用js来检查当前系统cpu的占用比例,通过 setTimeout 的方式探测 CPU 的大小，这样可以实现网页游戏中动画等耗时操作的自动调节。这个原理是很多人都知道的,就是用JS来踩点.

```js
var data = []
var t

function pulse() {
  t && data.push(Date.now() - t)  
  t = Date.now()  
  setTimeout(pulse, 50)
}

pulse()
```

就是每隔 50ms 打一下点。理想情况下，data 的值应该是

data = [50, 50, 50, 50, ...]

但实际情况，data 会是

data = [51, 52, 50, 52, ...]

当 CPU 比较忙时，data 的数据变成

data = [81, 102, 90, 62, ...]

即 CPU 越忙，data 数据项会越大。这样，记录一系列 data 值，就可以绘制出 CPU 占比趋势图，和通过任务管理器看到的 CPU 趋势图非常接近。

采用这样的思路,用js来画出cpu图片只需要下面的条件:
1 data数据不能无限增长,要控制数组长度.于是就用Array.shift()来控制
2 学习chartjs这个开源库,很简单,看看chartjs的文档就行了.chartjs中文文档
3 用自己的智慧.(饿,是显而易见的操作).来写个简单的实现

以下就是实现代码:
```js
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <meta name="author" content="marchen" />
    <title>cpu踩点图</title>
    <script src="http://cdn.bootcss.com/Chart.js/0.2.0/Chart.min.js"></script>
</head>

<body>
    <header>
        <h1>CPU踩点图</h1></header>
    <canvas id="myChart" width="1000" height="800" style="border:1px solid red">
        dont support canvas!
    </canvas>
    <script>
    var ctx = document.getElementById("myChart").getContext("2d");
    var data = null; //数据结构
    var timer = []; //x轴数字
    var i = 0; //迭代数字
    var info = []; //绘图信息
    var timeInt = 50; //时间间隔ms
    var timerNum = 20 //x轴参数个数
    var t; //打点取值时间
    var op = {
        scaleOverride: true,
        //** Required if scaleOverride is true **
        //Number - The number of steps in a hard coded scale
        scaleSteps: 40,
        //Number - The value jump in the hard coded scale
        scaleStepWidth: 1.5,
        //Number - The scale starting value
        scaleStartValue: 50,
        animation: false
    }

    function drawCpu() {
        //打点取值
        t && info.push(Date.now() - t);
        t = Date.now();
        if (info.length > timeInt) info.shift(); //控制数组长度
        //先前处理
        timer.push(i++);
        if (timer.length > timerNum) timer.shift();
        data = {
            labels: timer,
            datasets: [{
                fillColor: "rgba(40,20,0,0.5)",
                strokeColor: "rgba(220,220,220,1)",
                data: info
            }]
        }

        new Chart(ctx).Bar(data, op);
        setTimeout("drawCpu()", timeInt);
    }
    drawCpu();
    </script>
</body>

</html>

```

So,javascript能干什么?我想这个问题不用问了吧,虽然他的运行速度没有c那么快,但是编程的思想是一样的,c能做的事,为何js不能做呢?还可以试着用js来检测电脑性能,可以用来做很多有趣的事情.

掌握一门语言很幸福.

你能想着,我会编程,别人不会,那是多美的事情?do u think so?