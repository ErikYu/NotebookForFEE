### 3.2 jQuery中的DOM操作
---
#### 3.2.1 查找结点

1. 查找元素节点

````javascript
var $li=$("ul li:eq(1)");    //获取ul中的第二个li元素
var li_text=$li.text();      //获取这个元素节点的文本内容
````

2. 查找属性节点

````javascript
var $p=$("#p1");                 //获取id为p1的元素
var p_title=$p.attr("title");    //获取元素节点属性title
````
#### 3.2.2 创建节点

1. 创建元素节点

````javascript
var $li=$("<li></li>");    //$( html );工厂函数$()
````
这里跟原生javascript进行对比：
````javascript
//原生JS
var li=document.createElement("p");  //createElement()方法
````

2. 创建文本节点

创建元素节点的同时把文本内容写出来；
````javascript
var $li=$("<li>香蕉</li>");
$("#ul").append($li);
````
与原生JS进行对比：
````javascript
var liTxt=document.createTextNode("香蕉")
````

3. 创建属性节点

直接在建立元素节点的时候创建；
````javascript
//原生JS
li.setAttribute("id", "list");    //setAttribute()方法
````
#### 3.2.3 插入节点
jQuery中有以下几种插入节点的方法：

|方法|描述|实例|
|:---|:---|:---|
|append()|向每个匹配的元素**内部**追加内容|$("p").append("<em>你好</em>")|
|appendTo()|
|prepend()|想每个匹配的元素**内部**前置内容|
|prependTo()|
|after()|在每个匹配的元素之后插入内容|
|insertAfter()|
|before()|在每个匹配的元素之前插入内容|
|insertBefore()|

对比原生JS中的插入实现：

1. appendChild()方法：

parent.appendChild(child); //类似于jQuery中的append()方法；

2. insertBefore()方法：

结构：parentElement.insertBefore(newElement, targetElement)  //在targetElement之前插入newElement，父元素是parentElement

3. insertAfter()方法：

无此方法，需要定义：
````javascript
function insertAfter(newElement, targetElement){
    var parent=targetElement.parentNode;
    if(parent.lastChild == targetElement){
        parent.appendChild(newElement);
    } else{
        parent.insertBefore(newElememt, targetElement.nextSibling)
    }
}
````

#### 3.2.4 删除节点

1. remove()方法

该方法的返回值是一个指向已被删除的节点的引用，因此可以在以后继续使用中这个元素；
也可以通过传递参数来选择性的删除元素；
````javascript
var liRemoved=$("ul li").remove();   
var liRemoved2=$("ul li").remove("li[title!=菠萝]");  //将<li>中属性title不是菠萝的li元素删除
````

2. detach()方法
 
与remove()方法区别于：所有绑定的事件，附加的数据都会保留下来；
````javascript
$("ul li").click(function(){
    alert($(this).html());
})
var li=$("ul li:eq(1)").detach();
li.appendTo("ul");  //重新追加此元素，发现他之前绑定的事件仍存在
//使用remove()方法会导致之前绑定的事件失效
````

3. empty()方法

empty()方法不是删除节点，而是清空节点，清空元素内的所有后代元素；

4. 对比原生JS实现

removeChild()方法
````javascript
var formerFirstChild=parentNode.removeChild(parentNode.firstChild);
//移除第一个子节点，方法的返回值是一个指向被删除节点的引用
````
#### 3.2.5 复制节点
clone()方法
````javascript
$("").clone().appendTo();    //复制出来的元素不具备任何行为
$("").clone(true).appendTo();//复制出来的元素具有行为
````
原生JS实现复制节点：
cloneNode()方法：创建**调用这个方法的节点的**一个完全相同的副本，复制出来的元素属于文档，但是没有父节点：
````html
<ul>
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
</ul>
````
````javascript
var list=document.getElementsByTagName("ul");
var myList=list[0];
var deepList=myList.cloneNode(true);     //深复制，复制节点和整个子节点树
console.log(deepList.childNodes.length);       // 3(ie<9) / 7(其他浏览器)
var shallowList=mylist.cloneNode(false); //浅复制，只复制节点本身
console.log(shallowList.childNodes.length);    // 0
````
cloneNodes()方法不会复制添加到DOM节点中的JS属性，只复制特性和子节点。

#### 3.2.6 替换节点

1. replaceWith()和replaceAll()方法

替换后原先绑定的事件会跟着被替换的元素一起消失；
````javascript
$("p").replaceWith("<i></i>");    //将匹配的元素替换成指定的HTML和DOM元素
$("<i></i>").replaceAll("p");     //replaceWith的反操作
````

2. 原生JS实现，replaceChild()方法，使用replaceChild()插入一个节点时，这个节点的所有关系指针都会从被它替换的节点复制过来。

`parentNode.replaceChild(newNode, parentNode.childNode)`
````javascript
var returnNode=someNode.replaceChild(newNode, someNode.firstChild);  //
````

#### 3.2.7 包裹节点

1. wrap(), wrapAll(), wrapInner()

````javascript
$("p").wrap("<b></b>");                 //应用<b>标签把p包裹起来，<b><p></p></b>
$("p").wrapAll("");
$("p").wrapInner("<strong></strong>");  //将匹配元素的子内容用其他标签包裹，<p><strong></strong></p>
````

2. 原生JS实现
例如 : $('span').wrap('<div>');
先创建div元素,然后insertBefore到span的前面，再把span appendChild到div中,（这也是JQ的做法）
````javascript
function wrap(elem, label){
    var newElem=document.createElement(label);
    elem.parentNode.insertBefore(newElem, elem);
    newElem.appendChild(elem);
}

wrap(elem, "strong");
````
#### 3.2.8 属性操作

1. attr()方法：获取和设置属性

````javascript
var p_text=para.attr("title");    //获得para元素的title属性值
$("p").attr({"title": "myTitle", "name": "test"});    //设置属性，属性以对象格式传入，都需要引号
````
原生JS实现获取和设置属性：getAttribute()和setAttribute()方法
````javascript
object.getAttribute(attribute);        //只能通过元素节点对象调用
object.setAttribute(attribute, value); //只能用于元素节点
````
getAttribute()不是document对象，不能通过document对象调用。

2. removeAttr()方法：删除属性

````javascript
$("p").removeAttr("title");    //删除p元素的title属性
````

3. prop()和removeProp()；

对于有些属性没有属性值，eg`<input required />`，此时应该用`.prop("required")`来获取属性。
#### 3.2.9 样式操作
style属性不能获取外部样式表的内容，通过设置元素的class属性来操作样式，行为和表现分离。
addClass(): 
removeClass(): 
toggleClass(): 切换样式
hasClass(): 判断是否有某个属性
````javascript
$("p").addClass("one");     //给元素p增加一个one的class
$("div").removeClass("two");//为元素div移除two这个class，没有参数则移除全部class
$("p").toggle("three");
if(para.hasClass("grid"));  //判断para是否有grid这个class
````
原生JS实现addClass()，传递两个参数，元素和类名。
````javascript
function addClass(elem, value){
    if(!elem.className){
        elem.className=value;             //原className为空则直接设置
    } else{
        var newClassName=elem.className;  //获取原className
        newClassName+=" "+value;          //两个className之间用空格隔开
        elem.className=newClassName;
    }
}
addClass(para, "color");                  //为para增加color这个class
````
#### 3.2.10 设置和获取HTML，文本和值

1. html()，读取和设置元素内部的html代码，可用于XHTML，不能用于XML；

原生JS：innerHTML

2. text()，读取和设置元素内部的文本内容，可用于XHTML和XML；

原生JS：innerText

3. val()，设置和获取元素的值

用于select，checkbox，radio选取
````javascript
$("#single").val("选择1号");     //根据value属性选取相应
$("#multiple").val(["选择2号", "选择3号"]);   //以数组形式传递
$(":checkbox").val(["check1", "check2"]);    //多选框
````
原生JS：value
#### 3.2.11 遍历节点

1. children()方法：获取匹配元素的子元素集合，只考虑子元素而不考虑其他后代元素。
2. next()方法：取得匹配元素后面紧邻的同辈元素。
3. prev()方法：取得匹配元素前面紧邻的同辈元素。
4. siblings()方法：取得匹配元素前后所有的同辈元素。
5. closest()方法：取得最近的匹配元素：

- 检查当前元素是否匹配，如果匹配直接返回元素本身；
- 不匹配则向上查找父元素，直到找到匹配选择器的元素；
- 如果没找到则返回一个空的jQuery对象。

6. parent(), parents(), closest()区别

|方法|描述|实例|
|:--|:--|:--|
|parent()|获得集合中每个匹配元素的父级元素|
|parents()|获得集合中每个匹配元素的祖先元素|
|closest()|获得匹配的最近的祖先元素|

7. 除此之外，jQuery还有其他一些遍历节点的方法，find(), filter(), nextAll(). prevAll()等，这些遍历DOM方法都可以使用jQuery表达式作为参数来筛选元素。

#### 3.2.12 CSS-DOM操作
style属性无法获取外部css样式表的信息，在jQuery中，可以直接操作css，利用css()方法：
````javascript
$("p").css("color");     //获取p的样式颜色
````
和attr()一样，可以同时设置多个样式属性：
````javascript
$("p").css({"color": "red", "fontFamily": "Arial"})
````
.css("height")和.height()的区别：
.css("height"): 获取的高度值与设置的样式有关，有可能是auto，也可能是10px之类的字符串；
.height(): 获取的高度是元素在页面中的实际高度，不包含任何单位；
在css-dom中，关于元素定位，有以下几个常用的方法：

1. offset()方法：获取元素在当前视窗中的相对偏移，包含top、left两个属性，只对可见元素有效：

````javascript
var $offset=$("#para1").offset()
var topVal=$offset.top;
var leftVal=$offset.left;
````
原生JS中，没有类似于jQ中offset()的方法，offsetTop是相对于父节点的相对偏移，可以通过逐级累加的方式获取相对于视窗的offset值：
````javascript
function getOffset(node, offset){
    if(!offset){                               //创建一个空的offset对象
        var offset={};
        offset.top=0;
        offset.left=0;
    }
    if(node==document.body){                   //node到body时，结束。
        return offset;
    }
    offset.top+=node.offsetTop;                 //逐级叠加
    offset.left+=node.offsetLeft;
    return getOffset(node.parentNode, offset);  //调用自己
}

getOffset(a).top;
````

2. position()方法：获取元素相对与最近的absolute或者relative的祖父节点的相对偏移，包含top、left两个属性；
3. scrollTop()和scrollLeft()：获取元素的滚动条距顶端和左边的距离；

#### 附录：
##### 原生JS中的一些常用DOM方法：
---
|方法|描述|
|:--|:--|
|getElementById()|返回带有指定 ID 的元素。|
|getElementsByTagName()|返回包含带有指定标签名称的所有元素的节点列表（集合/节点数组）。|
|getElementsByClassName()|返回包含带有指定类名的所有元素的节点列表。|
|appendChild()|把新的子节点添加到指定节点。|
|removeChild()|删除子节点。|
|replaceChild()|替换子节点。|
|insertBefore()|在指定的子节点前面插入新的子节点。|
|createAttribute()|创建属性节点。|
|createElement()|创建元素节点。|
|createTextNode()|创建文本节点。|
|getAttribute()|返回指定的属性值。|
|setAttribute()|把指定属性设置或修改为指定的值。|