CHPT7--变换和动画
===
CSS3中的变换属性：transform
CSS3中的渐变效果：transition
### 7.1 CSS3的变换类型
---
注：transform的兼容性如下
- IE10、Firefox、Opera支持transform属性；
- IE9支持替代的-ms-transform属性，仅适用于2D转换；
- Safari和Chrome支持替代的-webkit-transform属性；
- Opera只支持2D转换；

#### 7.1.1 rotate旋转变换

1. 最简单的2D旋转

````css
div{
    transfrom: rotate(7deg);
    -ms-transform: rotate(7deg);    /*IE9*/
    -moz-transform: rotate(7deg);   /*Firefox*/
    -webkit-transform: rotate(7deg);/*safari和Chrome*/
    -o-transform: rotate(7deg);     /*Opera*/
}
````
rotateX, rotateY, rotateZ: rotateZ相当于rotate
如果要在其他向量上旋转，可以使用rotate3d(x, y, z, deg)，xyz的值建立三维向量，deg则是旋转角度。
#### 7.1.2 skew扭曲变换
````css
div{
    transform: skew(20deg, 10deg);    /*在X轴方向偏转20°，Y轴方向偏转10°*/
}
````
注意：skew没有3D和skewZ选项
#### 7.1.3 scale比例缩放
````css
div{
    transform: scale(1.1, 1.1);
}
````
注：可以使用scaleX, scaleY, scaleZ进行单一方向上的缩放。
#### 7.1.4 translate位移变换
````css
div{
    transform: translate(100px, 20px);  /*在x方向移动100px，Y方向移动20px*/
}
````
注：可以使用translateX, translateY, translateZ进行单一方向上的位移。
### 7.2 使用transition制作交互动画
---
用jquery实现动画效果：
````javascript
$(element).animate({width: "200px"}, 3000);
//$().animate(params, time, callback)
````
CSS3中的transition属性可以平滑改变CSS属性值
````css
.content{
    height: 100px;
    width: 100px;
    -webkit-transition: height 600ms;
    -moz-transition: height 600ms;
    -o-transition: height 600ms;
    transition: height 600ms;
}
.content:hover{
    height: 300px;
}
````
上例即为高度为100px的正方形在hover下0.6s内变为300px的动画。如果需要改变多个属性，可以使用逗号隔开：
````css
.content{
    transition: height 2s, width 2s, background 2s;
}
````
transition还可以包含设置渐进动画的函数，可以选择的函数有6种。
- ease: 匀速变慢
- linear: 匀速
- ease-in: 加速
- ease-out: 减速
- ease-in-out: 加速然后减速
- cubic-bezier: 自定义时间曲线

````css
transition: all 0.5s ease-in-out 1s;
````
四个参数依次表示：属性、过渡时间、过渡函数、延迟时间
### 7.3 使用`@keyframes`制作动画(关键帧)
---
#### 7.3.1 `@keyframes`的基本语法
````css
@keyframes spin{
    from{
        -webkit-transform: rotateY(0);
    }
    to{
        -webkiy-transform: rotateY(-360deg);
    }    /*from和to代表0%和100%*/
}

@keyframes spin{
    0% {
        -webkit-transform: rotateY(0);
    }
    50% {
        -webkit-transform: rotateY(-180deg);
    }
    100% {
        -webkit-transfor,: rotateY(-360deg);
    }
}
````
`@keyframes`必须配合元素中定义的animation属性，用于定义动画
````css
animation: spin 8s infinite linear alternate;
````
spin: 动画名称
8s：动画执行一次所需要的时间
infinite: 动画执行的次数
linear: 动画的速度函数，跟transition的速度函数相同
alternate: 表示动画正向循环完毕后反向循环
如果想对动画的运行进行控制，可以给元素增加animation-play-state属性：
````css
div{
    animation-play-state: paused;  /*paused为暂停*/
    animation-play-state: running; /*running为开启动画*/
}
````
可以通过js控制这个属性来完成。
#### 7.3.2 实例

````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            div{
                width: 100px;
                height: 100px;
                background-color: red;
                position: relative;
                animation: firstAni 5s infinite;
                -webkit-animation: firstAni 5s infinite ;
            }
            @keyframes firstAni{
                0% {background: red; left: 0px; top: 0px;}
                25% {background: yellow; left: 200px; top: 0px;}
                50% {background: blue; left: 200px; top: 200px;}
                75% {background: green; left: 0px; top: 200px;}
                100%{background: red; left: 0px; top: 0px;}
            }
            @-webkit-keyframes firstAni{  /*适用于safari和chrome*/
                0% {background: red; left: 0px; top: 0px;}
                25% {background: yellow; left: 200px; top: 0px;}
                50% {background: blue; left: 200px; top: 200px;}
                75% {background: green; left: 0px; top: 200px;}
                100%{background: red; left: 0px; top: 0px;}
            }
        </style>
    </head>
    <body>
        <div></div>
    </body>
</html>
````
#### 7.3.4 `@keyframes`小结
- keyframes可以改变任意多的样式，任意多的次数；
- 使用百分比来规定变化发生的时间，或者用from, to；
- 为了得到最佳的浏览器支持，应始终定义0%和100%选择器

注：IE10、Firefox、opera支持`@keyframes`和animation属性；Chrome和Safari需要增加前缀-webkit-；IE9及之前不支持。
### 7.4 实例
---
````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            .wahaha{
                width: 100px;
                height: 100px;
                text-align: center;
                background: #CCCCCC;
                line-height: 100px;
                font-family: "microsoft yahei";
                font-size: 50px;
                animation: rotateYdir 3s infinite alternate;
                -webkit-animation: rotateYdir 3s 2 alternate;
                animation-play-state: paused;
                -webkit-animation-play-state: paused;
                border-radius: 10px;
            }
            @keyframes rotateYdir{
                0%{transform: rotateY(0);}
                100%{transform: rotateY(-360deg);}
            }
            @-webkit-keyframes rotateYdir{
                0%{-webkit-transform: rotateY(0);}
                100%{-webkit-transform: rotateY(-360deg);}
            }
            .yunxing{
                animation-play-state: running;
                -webkit-animation-play-state: running;
            }
        </style>
    </head>
    <body>
        <div class="wahaha">6</div>
        <script>
            var div1=document.getElementsByClassName("wahaha");
            div1[0].onmouseover=function(){
                this.style.animationPlayState="running";
                this.style.webkitAnimationPlayState="running"
            }
        </script>
    </body>
</html>
````
### 7.6 小结
---
- 元素的变换：应用transform属性可以对元素进行旋转rotate，扭曲skew，位移translate，缩放scale；
- 元素样式改变的过渡效果，应用transition属性可以改变和添加过渡效果；几个速度函数：ease, ease-in, ease-out, ease-in-out, linear内置函数：`transition: prop 0.5s linear 1s`,prop是css属性名；
- 使用`@ketframes`和animation属性设置动画循环。