---
title: Canvas躲避球游戏
date: 2018-5-9
categories: Web
tags: [Canvas,JavaScript]
---

## 总体说明

上上周接触的canvas绘图, 觉得很是欢乐, 之前也记得腾讯斗地主的web版本也是整个页面一整个canvas. 而且canvas性能比DOM强多了, 做游戏应该会很有意思. 前两天写的一个躲避球的游戏, 在canvas上随机生成的小球, 随机的颜色, 随机的运动, 带有碰撞检测(原来大学物理是有用的!). 游戏内容就是根据躲避球的时间长短计分. 分数记录在localStorage.

## 实现方式

在线体验: https://limbowandering.github.io/web/game/canvasBallV1.html

先说句题外话, 昨天就该写这篇博客的, 但是太累了, 12点才有时间, 就睡了. 果然博客就应该在脑子热的时候写,现在写不动了, 好难受. 

先上代码吧:

HTML&CSS

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Canvas Game</title>
    <style>
        body {
            padding: 0;
            margin: 0;
            overflow: hidden;
        }
        #cas {
            display: block;
        }
    </style>
    <script src="./scripts.js"></script>
</head>
<body>

<div>
    <canvas id='cas' width="1200" height="600" style="background-color:#000000">浏览器不支持canvas</canvas>
</div>

</body>
</html>
```

然后一点点来分析JavaScript的代码:

### 定义Ball类

JS面向对象, 并不是完全的面向对象, 还是有区别的, 但是定义构造函数和对象的方法还是可以的. 

``` JavaScript
//let getRandom_v = () => 10*(parseInt(Math.random() * 5 - 2) || 2);
//两次位运算取否， 得到整数部分 
//上面我还禁止了速度为0的情况，但是还是允许吧 反正随机
let getRandom_v = () => 20* ~~(Math.random() * 5 - 2);
let Ball = function(x,y,r,color){
    this.x = x;
    this.y = y;
    //这个速度回头再调整
    //可能还需要重构一下, 用于游戏难度自定义
    this.vx = getRandom_v();
    this.vy = getRandom_v();
    this.radius = r;
    this.color = color;
};
Ball.prototype = {
    paint: function(){
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius - 1, 0, Math.PI * 2);
        ctx.fillStyle = this.color;
        ctx.fill();
        //边框就不画了. 这种canvas像素的操作,碰撞检测时候也是蛮揪心的.
        //ctx.strokeStyle = '#fff';
        //ctx.stroke();
    },
    run: function(t){
        //目前只是匀速运动, 之前有看到过模拟重力的, 加速度什么的, 原来物理还是有用的
        this.x += this.vx * t;
        this.y += this.vy * t;
        //debugger;
        //y/x方向上与边框的碰撞检测
        if (this.y > canvas.height - ballRadius || this.y < ballRadius) {
            this.y = this.y < ballRadius ? ballRadius : (canvas.height - ballRadius);
            this.vy = -this.vy;
        }
        if (this.x > canvas.width - ballRadius || this.x < ballRadius) {
            this.x = this.x < ballRadius ? ballRadius : (canvas.width - ballRadius);
            this.vx = -this.vx;
        }
        //接收的参数t是时间, 每t时间更新一下位置, 重新绘制一下.
        //其实这里有问题, 往下看看requestAnimationFrame是根据显示器刷新频率来决定频率的。
        //所以这个t 不是真的t。。。只是用来更新位置用
        this.paint();
    },
}
```

额 写不下去这么多注释了 下面注释少写点

### 初始化

``` javascript
//window onload的时候更新canvas大小
canvas = document.getElementById('cas');
ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;
ctx.font = "20px Georgia";

//玩家的球
let myBall = new Ball(-500,-400,ballRadius,"white",0,0);
myBall.vx = 0;
myBall.vy = 0;
balls.push(myBall);

function getRandom(a,b){
    return parseInt(Math.random() * (b - a) + a);
}

//随机生成40个球
for(let i = 0; i < 40; i++){
    let ball = new Ball(getRandom(ballRadius, canvas.width - ballRadius),
                        getRandom(ballRadius, canvas.height - ballRadius),
                        ballRadius, "rgba(" + parseInt(Math.random() * 255)
                        + "," + parseInt(Math.random() * 255) + "," + parseInt(Math.random() * 255) + " , 1)");

    balls.push(ball);
}
```

### 让球动起来

``` JavaScript
function animate(){
    score += 1;
    //score的等会再说
    //先清除整个canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.save();
    ctx.fillStyle = "rgba(255,255,255,0.2)";
    ctx.fillRect(0,0,canvas.width, canvas.height);
    ctx.restore();
    ctx.clearRect(0, 0, canvas.width, canvas.height);
	//检测碰撞
    collision();
    //绘制当前得分
    ctx.fillText('score:'+score.toString(),10,50);
    //绘制最高分
    bestScore();
    //如果死了 
    if(balls[0].vx !== 0 || balls[0].vy !== 0){

        ctx.fillText('You died. Your score is '+score.toString(),(canvas.width/2)-100,canvas.height/2);
        drawButton((canvas.width/2)-40,(canvas.height/2)+30,100,35);
        ctx.fillStyle = '#fff';
        ctx.fillText('再来一次',(canvas.width/2)-30,(canvas.height/2)+53);
        updateScores();
        return;
    }
	//更新球的位置
    balls.forEach((b) => b.run(t));
    myBall.paint();
    //用requestAnimationFrame刷新
    if ("requestAnimationFrame" in window) {
        requestAnimationFrame(animate);
    } else if ("webkitRequestAnimationFrame" in window) {
        webkitRequestAnimationFrame(animate);
    } else if ("msRequestAnimationFrame" in window) {
        msRequestAnimationFrame(animate);
    } else if ("mozRequestAnimationFrame" in window) {
        mozRequestAnimationFrame(animate);
    }
    //webkitRequestAnimationFrame(animate);
}
```

#### 关于RequestAnimationFrame

顾名思义, 请求动画帧.  对于屏幕刷新率和动画原理就不讲了. 

用setTimeout如果在低端机上的话, 会出现卡顿, 抖动的现象. 这种现象的产生有两个原因:

- setTimeout的执行时间并不是确定的. 在JavaScript中, setTimeout任务被放进了异步队列中, 只有当主线程上的任务执行完以后, 才回去检查队列里的任务是否需要开始执行, 因此setTimeout的实际执行时间一般要比设定时间晚一点.
- 受到屏幕分辨率和屏幕尺寸的影响, 不同设备刷新频率可能会不同, 而setTimeout只能设置时间间隔, 和屏幕刷新时间不一定相同.

所以, setTimeout的执行步调与屏幕的刷新步调不一致就会导致**丢帧**的现象. 这个自己算一下就知道了.

与setTimeout相比, requestAnimationFrame最大的优势就是由系统来决定回调函数的执行时机. **它能保证回调函数在屏幕每一次的刷新间隔中只被执行一次.** 

而且requestAnimationFrame还有两个优势: 

- **CPU节能**：使用setTimeout实现的动画，当页面被隐藏或最小化时，setTimeout 仍然在后台执行动画任务，由于此时页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。而requestAnimationFrame则完全不同，当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统步伐走的requestAnimationFrame也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销。


- **函数节流**：在高频率事件(resize,scroll等)中，为了防止在一个刷新间隔内发生多次函数执行，使用requestAnimationFrame可保证每个刷新间隔内，函数只被执行一次，这样既能保证流畅性，也能更好的节省函数执行的开销。一个刷新间隔内函数执行多次时没有意义的，因为显示器每16.7ms刷新一次，多次绘制并不会在屏幕上体现出来。

对于有些浏览器尚不支持的情况, 有对应的polyfill在GitHub上.

### 碰撞检测

``` JavaScript
function collision(){
    for (let i = 0; i < balls.length; i++) {
        for (let j = 0; j < balls.length; j++) {
            let b1 = balls[i], b2 = balls[j];
            if (b1 !== b2) {
                let rc = Math.sqrt(Math.pow(b1.x - b2.x, 2) + Math.pow(b1.y - b2.y, 2));
                //向上舍入
                if (Math.ceil(rc) < (b1.radius + b2.radius + 2)) {
                    let ax = ((b1.vx - b2.vx) * Math.pow((b1.x - b2.x), 2) +
                              (b1.vy - b2.vy) * (b1.x - b2.x) * (b1.y - b2.y)) / Math.pow(rc, 2);
                    let ay = ((b1.vy - b2.vy) * Math.pow((b1.y - b2.y), 2) +
                              (b1.vx - b2.vx) * (b1.x - b2.x) + (b1.y - b2.y)) / Math.pow(rc, 2);

                    b1.vx = (b1.vx - ax) * collarg;
                    b1.vy = (b1.vy - ay) * collarg;
                    b2.vx = (b2.vx + ax) * collarg;
                    b2.vy = (b2.vy + ay) * collarg;
					
                    //这里有个坑, 如果仅仅改变速度就完蛋了, 因为再来一帧还有可能继续碰撞
                    //所以需要立马把两个球分开
                    let clength = ((b1.radius + b2.radius + 2) - rc) / 2;
                    let cx = clength * (b1.x - b2.x) / rc;
                    let cy = clength * (b1.y - b2.y) / rc;
                    b1.x = b1.x + cx;
                    b1.y = b1.y + cy;
                    b2.x = b2.x - cx;
                    b2.y = b2.y - cy;
                }
            }
        }
    }
}
```

### 计分

用到的都是localStorage最基本的setItem, getItem, length属性. 不作说明了.

完整的代码请移步: [GitHub](https://github.com/limbowandering/web_demos/tree/master/4_canvasBallV1) 

## 最后

对于canvas, SVG, WebGL, 都觉得很有趣, 接下来还会再玩玩. 

对于本游戏, 目前的JS代码加上注释才150行, 就感觉有点乱, 对于设计模式啊, 面向对象的设计, 还得琢磨. 而且对于在这个游戏, 我发现能做的太多了! 目前是躲避, 可以改成吃豆豆的… 还可以是双人的吃豆豆, 加上websocket通信, 对于延迟和检验什么的, 做了再说吧. 还要把界面做的更友好的一点, 第一次进入做一个游戏新手教程, 游戏还应该能设置难度, 还有自定义球的速度和数量. 而且还能加入音效. 接着慢慢做吧.

一些参考链接

[伯乐在线](http://web.jobbole.com/91578/)