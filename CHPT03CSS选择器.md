CHPT3--CSS选择器
===
### 3.1 基础选择器
---
#### 3.1.1 标签选择器
标签选择器一般用于定义全局样式；
bootstrap中关于标题的全局定义：
````css
h1,h2,h3,h4,h5,h6{
    margin: 10px, 0;
    font-family: inherit;
    font-weight: bold;
    line-height: 20px;
    color: inherit;
    text-rendering: optimizelegibility;
}
h1,h2,h3{
    leng-height: 40px;
}
h1{font-size: 38.5px;}
h2{font-size: 31.5px;}
h3{font-size: 24.5px;}
h4{font-size: 17.5px;}
h5{font-size: 14px;}
h6{font-size: 11.9px;}
````
#### 3.1.2 类选择器
又称class选择器；
> 注：从代码可读性和可维护性上说，最好不要给一个标签增加多余两个class属性；尽量为class指定有意义的名字。

#### 3.1.8 属性选择器
类似jQuery中的属性选择器，但有区别
````javascript
$("div[id=test]")
````
````css
div[id="test"]{
    /*style*/
}
````
使用通配符进行模糊匹配，语法跟jQuery选择器相同，区别在引号;
````css
a[src^="https"]    /*以https开头*/
a[src$=".pdf"]     /*以.pdf结尾*/
a[src*="abc"]      /*属性中包含abc字符串*/
````
#### 3.1.10 复合选择器
应用复合选择器时，标签选择器一定要写在最前面，否则无法识别

### 3.2 伪类选择器
---

1. :nth-child(n)，与jQuery的子元素选择器相同；
2. :nth-last-child(n)，与nth-child(n)类似，只是这里从最后一个元素开始计数
3. :nth-of-type(n)，类似于:nth-child(n)，区别在于：li:nth-child(3)，一旦第三个元素不是li元素，选择器就不会起作用；p:nth-of-type(3)查询的是第三个p元素；如果不加标签类型，会自动选择所有并列元素的第n个。
4. :nth-last-of-type(n)，类似于:nth-of-type，区别在于从最后一个开始计数
5. :last-child，选择元素的最后一个子元素。

> 注：:last-child是CSS3新增的伪类选择器，:first-child在CSS2种就已经加入了。IE6不支持:first-child，IE6-8不支持:last-child；

6. :only-child，与jQuery类似，如果父元素只有一个子元素，如果有限定条件吗，则取交集。 div p:only-child，选取div中唯一的子元素p
7. :only-of-type，基本等同:only-child，区别在于不加限定条件的情况下，:only-of-type会选取body元素；
8. :root，选取根元素，即html标签
9. :empty，选择没有任何内容的元素

#### 3.2.2 目标伪类:target
URL前面有锚名称#，指向文档内某个具体的元素，这个被链接的元素就是目标元素。:target选择器可用于选取**当前活动**的目标元素。
````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="{CHARSET}">
        <style>
            :target{
                background-color: #CCCCCC;
                border: 1px solid;
            }
        </style>
	</head>
	<body>
        <a href="#id1">id1</a>   <!--点击id1，目标位置变为样式表设置-->
        <a href="#id2">id2</a>
        <div id="id1">asd</div>
        <div id="id2">zxc</div>
        <div id="id3">qwe</div>
    </body>
</html>
````
#### 3.2.3 状态伪类
CSS3新增的伪类选择器，用于表示表单元素的状态，与jQuery中的表单对象属性过滤选择器相同；由于很多浏览器不支持CSS3，下面介绍如何用属性选择器代替状态伪类；

1. :enabled和:disabled

````css
input:disabled{}
input[disabled]  /*使用属性选择器达到状态伪类的相同效果*/
````

2. :checked

#### 3.2.4 否定伪类
:not( selector )
````css
div :not(.myClass){}     /*选取div的子元素中，class不是myClass的元素*/
````

### 3.4 小结
---
- 标签选择器主要用于定义全局样式
- 单一的类选择器不要滥用，比较容易出现命名冲突等
- 可以使用通配符进行一些全局设置
- 子元素选择器“>”和后代元素选择器“空格”的区别；
- 组选择器可以很好的缩减冗余代码；
- CSS3之前的伪类选择器有:link, :visuted, :hover, :active, :first-child, :lang
