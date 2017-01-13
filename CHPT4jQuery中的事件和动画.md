CHPT4--jQuery中的事件和动画
===
### 4.1 jQuery中的事件
---
#### 4.1.1 加载DOM
原生JS：window.onload方法，网页内所有的元素，包括所有的元素关联文件完全加载到浏览器后才会执行；
jQuery：$(document).ready()或者$(function())，DOM完全就绪就会被调用；
window.onload方法一次只能保存对一个函数的引用；
````javascript
window.onload=function(){
    one();
    two();
}
````
或者使用addLoadEvent()方法：
````javascript
function addLoadEvent(func){
    var oldOnload=window.onload;
    if(typeof(oldOnload)!="function"){
        window.onload=func;
    } else{
        window.onload=function(){
            oldOnload();
            func();
        }
    }
}
addLoadEvent(one);   //调用
addLoadEvent(two);
````
#### 4.1.2 事件绑定
bind()方法：
````javascript
$().bind(type[, data], fn)
//type: 事件类型
//data: 作为event.data属性值传递给事件对象的额外数据对象
//fn: 用来绑定的处理函数
````
这里介绍一个is()方法，根据选择器来检测匹配元素集合，如果其中至少一个表达式满足则返回true。
````javascript
if($(this).next().is(":visible")){   //如果内容显示
    $(this).next().show();
}
````
原生JS实现的事件绑定：
````javascript
var para=document.getElementById("para");
para.onclick=function(){
   //do something
};
````
#### 4.1.3 合成事件

1. hover()方法：用于模拟光标悬停事件，替代mouseenter和mouseleave事件。`hover(enter, leave)`

````javascript
$("p").hover(function(){
    $(this).next().show();
}, function(){
    $(this).next().hide();
})
````

2. toggle()方法：模拟鼠标连续单击事件。`$("p").toggle(fn1, fn2, fn3)`；

还可以切换元素的显示状态：`$(this).next().toggle()`
#### 4.1.4 事件冒泡

1. 什么是冒泡：元素嵌套的情况下，操作内层元素（如单击）时，也会操作外层元素，事件按照DOM层次结构像水泡一样不断向上直到顶端；
2. 事件对象：只有事件处理函数才能访问得到，事件处理函数执行完毕后，事件对象就被销毁；

````javascript
$("p").click(function(event){  //只需要给函数增加一个参数event
    
})
````

3. 停止事件冒泡：stopPropagation()方法

````javascript
$().bind("click", function(event){
//  do something
    event.stopPropagation();   //停止事件冒泡
})
````

4. 阻止默认行为：preventDefault()方法

````javascript
$().bind("click", function(event){
//  do something
    event.preventDefault();   //阻止默认行为
})
````
以上这两个都可以改写为return false;

5. 事件捕获

#### 4.1.5 事件对象的属性

1. event.type：获取事件的类型，click等等
2. event.preventDefault()：阻止事件的默认行为，原生JS中的preventDefault()在IE中无效，jQuery对其进行了封装，兼容；
3. event.stopPropagation()：阻止事件的冒泡，兼容性同上；
4. event.target：获取到触发事件的元素；
5. event.relatedTarget：返回与事件的目标节点相关的节点：

- 对于mouseover事件，该属性是鼠标移到目标节点上时所离开的那个节点；
- 对于mouseout事件，该属性是离开目标时，鼠标指针进入的节点

6. event.pageX和event.pageY：获取光标相对于页面的x坐标和y坐标，如果页面上有滚动条，还要加上滚动条的高度和宽度；

````javascript
$("p").bind("click", function(event){
    console.log(event.pageX);
})
````

7. event.which：鼠标单击事件中获取鼠标的左中右键；在键盘事件中获取按键；
8. event.metaKey：为键盘事件中获取ctrl按键

#### 4.1.6 移除事件
unbind()方法：`unbind([type][, data])`
````javascript
$("p").click(myFunc1=function(){
    //do something
}).click(myFunc2=function(){
    //do something
}).click(myFunc3=function(){
    //do something
})
$("p").unbind("click", myFunc2);
//移除click事件中的myFunc2
//这里需要给匿名事件处理函数指定一个变量名，才能指定删除
````
One()方法：结构与bind()类似，执行一次后，立即被删除，不再绑定元素
#### 4.1.7 模拟操作
trigger()方法：`trigger(type[, data])`；type可以自定义名称
````javascript
$("#htn").bind("myClick", function(event, mess1, mess2){
    $("#test").append("<p>"+mess1+mess2+"</p>");
})
$("#btn").trigger("myClick", ["我的"， "自定义"])；  //传递两个参数，并执行myClick这个自定义的事件
````
trigger()方法触发事件后，还会执行浏览器默认操作，要想不执行浏览器默认操作，可以使用triggerHandler()方法。
#### 4.1.8 其他用法

1. 绑定多个事件类型

当多个事件使用同一个事件处理函数时，可以使用bind()方法绑定多个事件类型：
````javascript
$("p").bind("mouseover mouseout", function(){
    $(this).toggleClass("over");               //给mouseover和mouseout设置同一个事件处理函数
})
````

2. 添加事件命名空间，便于管理

type.zoneName -> click.plugin
````javascript
$("p").bind("click.plugin", function(){
//  do something
})
$("p").bind("mouseover.plugin", function(){
//  do something
})
$("p").bind("dbclick", function(){
//  do something
})
$("p").unbind(".plugin");    //plugin命名空间内的事件会被删除
````

3. 相同事件名称，不同命名空间执行方法

````javascript
$("div").bind("click", function(){
    $(this).append("<p>click<p>")
}).bind("click.plugin", function(){
    $(this).append("<p>click.plugin</p>")
})
$("div").trigger("click!");
````
**这一条最近的jQuery已经移除！！！！！**
看着玩玩吧
### 4.2 jQuery中的动画
---
#### 4.2.1 show()和hide()方法
show()和hide()就是切换元素的display样式，hide()设置为none，并记住原先的属性值，show()会切回原先的样式值。
指定参数可以控制显示和消失的速度：
````javascript
$("div").hide("slow");  //600ms
$("div").hide(1000);    //1000ms
````
#### 4.2.2 fadeIn()和fadeOut()方法
fadeIn()和fadeOut()只改变元素的不透明度，fadeOut()就是降低不透明度，直到display: none;
#### 4.2.3 slideUp()和slideDown()方法
slideUp()和slideDown()只会改变元素的高度。如果一个元素的display为none，调用slideDown()时，这个元素将自上而下显示
#### 4.2.4 自定义动画animate()方法
结构：`animate(params, speed, callback)`
params：一个包含样式属性及值得映射，比如{property1: "value1", property2: "value2"}
累加累减动画和同时执行多个动画：
````javascript
$("p").animate({left: "+=100px", top: "+=150px"}, 2000);   //2000ms内完成一次，可以累加运行，即每次运行在上一次运行的基础上
````
按顺序依次执行多个动画：
````javascript
$("p").animate({left: "200px"}).animate({top: "100px"})
````
#### 4.2.5 动画回调函数callback
回调函数在执行完动画之后执行，如果想在执行完动画后修改css样式，链式操作增加css()并不会有预期效果，因为css()会被立刻执行，不加入动画队列。
应当把css()写到最后一个动画的回调函数中。
````javascript
$(this).animate({left: "100px"}, 200)
.animate({top: "100px"}, 200, function(){
    $(this).css("color","#cccccc")
})
````
#### 4.2.6 停止动画和判断是否处于动画状态
**stop()方法**：`stop([clearQueue], [gotoEnd])`
clearQueue：可选值，判断是否清空未执行完的动画队列；
gotoEnd：是否直接将正在执行的动画跳转到末状态；
jQuery未提供直接到达动画队列最后的方法；
**.is(":animated")**：判断是否处于动画状态：
`.is( selector )`，返回一个布尔值，判断。
**delay()方法**：将队列中的函数延迟执行：
`$(element).delay(1000)`
````javascript
$("p").stop(true, true);    //清空未执行完的动画队列，并跳转到当前动画的末状态
if($("p").is(":animated")){ //判断p元素是否处于动画状态
//  do something
}
````
#### 4.2.7 其他动画方法

1. `toggle(speed, [callback])`：切换元素的显示状态，相当于show()和hide()的切换；
2. `slideToggle(speed,[easing], [callback])`：通过高度变化切换元素的可见性；
3. `fadeTo(speed, opacity, [callback])`：不透明度以渐进方式调整到指定值；
4. `fadeToggle(speed, [easing], [callback])`：通过调整不透明度切换显示状态。

#### 4.2.8 animate()代替其他动画方法
````javascript
$(elem).show(400);
$(elem).animate({height: "show", width: "show", opacity: "show"}, 400)

$(elem).slideDown(400);
$(elem).animate({height: "show"}, 400);

$(elem).fadeIn(400);
$(elem).animate({opacity: "show"}, 400);

$(elem).fadeTo(400, 0.6);
$(elem).animate({opacity: "0.6"}, 400)
````
注意：在动画方法中，非动画方法会插队，要让非动画方法也按顺序执行，需要把他写在回调函数中。
### 4.3 原生JS中的动画基础
---
#### 4.3.1 位置和时间
位置：设置position为absolute或者relative后修改top和left属性实现位置更改；
时间：setTimeout()方法，让函数经过一段预定时间后再运行
取消：clearTimeout()方法，取消等待执行队列中的函数
````javascript
var variable=setTimeout("function", interval);
clearTimeout(variable);     //取消等待执行队列中的函数
````
#### 4.3.2 时间递增量
每隔一段时间执行一小段动画：
````javascript
function moveMessage(){
    var elem=document.getElementById("message");
    var xpos=parseInt(elem.style.left);    //必须使用DOM脚本或style属性为元素分配位置之后，这里的parseInt才有作用。
    var ypos=parseInt(elem.style.top);
    if(xpos==200 && ypos==100){
        return true;
    }
    if(xpos<200){
        xpos++;
    }
    if(xpos>200){
        xpos--;
    }
    if(ypos<100){
        ypos++;
    }
    if(ypos>100){
        ypos--;
    }
    elem.style.left=xpos+"px";
    elem.style.top=ypos+"px";
    movement=setTimeout("moveMessage()", 10);  //10ms后执行一次
}
//执行此函数
window.onload=function(){
    var elem=document.getElementById("message");
    elem.style.position="absolute";
    elem.style.top="0px";    //使用DOM脚本或style属性为元素分配位置。
    elem.style.left="0px";
    moveMessage();
}
````
#### 4.3.3 抽象化moveMessage() -> moveElement()
````javascript
window.onload=function(){
    positionMessage();
}
function moveElement(elemID, final_x, final_y, interval){
    if(!document.getElementById) return false;                   //检测
    if(!document.getElementById(elemID)) return false;
    var elem=document.getElementById(elemID);
    var xpos=parseInt(elem.style.left);                          //style属性返回的是字符串，parseInt()转化为整形
    var ypos=parseInt(elem.style.top);
    if(xpos==final_x && ypos==final_y) return true;
    if(xpos<final_x){xpos++};
    if(xpos>final_x){xpos--};
    if(ypos<final_y){ypos++};
    if(ypos>final_y){ypos--};
    elem.style.top=ypos+"px";
    elem.style.left=xpos+"px";
    var func="moveElement('"+elemID+"', "+final_x+", "+final_y+", "+interval+")";   //拼接
    movement=setTimeout(func, interval);
}
function positionMessage(){
    if(!document.getElementById) return false;                   //检测
    if(!document.getElementById("message")) return false;
    var elem=document.getElementById("message");
    elem.style.position="absolute";
    elem.style.top="0px";
    elem.style.left="0px";
    moveElement("message", 200, 100, 10);
}
````
### 4.4 原生JS中实用的动画
---
#### 4.4.1 一种轮播图的设计思路
jQuery实现
````html
<!DOCTYPE html>
<html>
<style>
*{margin: 0; padding: 0;}
.slideShow{
    width: 100px;
	height: 100px;
	position: relative;
	overflow: hidden;
}
.pin{
	width: 500px;
	position: absolute;
}
.box{
	width: 100px;
	height: 100px;
	text-align: center;
	float: left;
}
.one{background-color: #CCCCCC;}
.two{background-color: #ADD8E6;}
.three{background-color: #FFCCCC;}
.four{background-color: #E7E7E7;}
.five{background-color: #2B93D2;}
p{
	padding-top: 10px;
	width: 80px;
	border: 1px solid #ccc;
}
</style>

<body>
<div style="height: 200px;">
<div class="slideShow">
    <div class="pin">
        <div class="box one">one</div>
        <div class="box two">two</div>
		<div class="box three">three</div>
		<div class="box four">four</div>
		<div class="box five">five</div>
    </div>
</div>
</div>
<p id="button1">button1</p>
<p id="button2">button2</p>
<p id="button3">button3</p>
<p id="button4">button4</p>
<p id="button5">button5</p>

<script>
var btn1=$("#button1");
var btn2=$("#button2");
var btn3=$("#button3");
var btn4=$("#button4");
var btn5=$("#button5");
btn1.click(function(){
    if(!$(".pin").is(":animated")){
        $(".pin").animate({left: "0px"}, 200)
    }
})
btn2.click(function(){
    if(!$(".pin").is(":animated")){
        $(".pin").animate({left: "-100px"}, 200)
    }
})
btn3.click(function(){
    if(!$(".pin").is(":animated")){
        $(".pin").animate({left: "-200px"}, 200)
    }
})
btn4.mouseover(function(){
    if(!$(".pin").is(":animated")){
        $(".pin").animate({left: "-300px"}, 200)
    }
})
btn5.click(function(){
    if(!$(".pin").is(":animated")){
        $(".pin").animate({left: "-400px"}, 200)
    }
})
</script>
</body>
</html>
````
javascript实现
````javascript
function moveElement(elemId, final_x, final_y, interval){
    var elem=document.getElementById(elemId);
    if(elem.movement){                                      //如果正在执行动画，则将其取消
        clearTimeout(elem.movement);                        //clearTimeout()取消等待执行队列中的函数
    }
    if(!elem.style.left){                                   //必须用DOM设置left和top属性
        elem.style.left="0px";
    }
    if(!elem.style.top){
        elem.style.top="0px";
    }
    var xpos=parseInt(elem.style.left);
    var ypos=parseInt(elem.style.top);
    var dist=0;
    if(xpos==final_x && ypos==final_y) return true;
    if(xpos<final_x){
        dist=Math.ceil(Math.abs(final_x-xpos)/10);
        xpos+=dist;
    }
    if(xpos>final_x){
        dist=Math.ceil(Math.abs(final_x-xpos)/10);
        xpos-=dist;
    }
    if(ypos<final_y){
        dist=Math.ceil(Math.abs(final_y-ypos)/10);
        ypos+=dist;
    }
    if(ypos>final_y){
        dist=Math.ceil(Math.abs(final_y-ypos)/10);
        ypos-=dist;
    }
    elem.style.left=xpos+"px";
    elem.style.top=ypos+"px";
    var func="moveElement('"+elemId+"', "+final_x+", "+final_y+", "+interval+")";
    elem.movement=setTimeout(func, interval);
}
````