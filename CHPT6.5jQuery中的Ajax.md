CHPT6--jQuery和Ajax应用
===
### 6.5 jQuery中的Ajax
---
jQuery对Ajax进行了封装，$.Ajax()是最底层的方法；
第二层是load(), $.get(), $.post()方法；
第三层是$.getScript和$.getJSON方法；
#### 6.5.1. load()方法
从web服务器上获取静态的数据文件
1. 载入HTML文档

　　结构：**load(url[, data][, callback])**
　　url：请求HTML页面的URL地址
````html
<input type="button" id="send" value="Ajax获取" />
<div class="comment">已有评论</div>
<div id="resText></div>
````

````Javascript
$(function{
    $("#send").click(function(){
        $("#resText").load("test.html");
    });
});
````
　　以上.load()方法将test.html加载到主页面的div中

2. 筛选载入的HTML文档

　　load()方法中url参数的结构是"url selector"，例如"test.html .test"选取文档中的class为test的内容；
````javascript
$("#resText").load("test.html .para");
````

3. 传递参数

　　load()方法的传递参数的方法根据data自动选择，如果没有参数传递，使用GET方法；如果有参数传递，使用POST方法。

4. 回调函数

　　load()方法的callback有三个参数，分别代表请求返回的内容、请求状态、XMLHttpRequest对象；
````javascript
$("#resText").load("test.html", function(responseText, textStatus, XMLHttpRequest){
    // responseText: 请求返回的内容
    // textStatus: 请求状态，success error notmodified timeout四种
    // XMLHttpRequest: 对象
})
````
在Ajax中，无论Ajax()请求是否成功，只要当请求完成（complete）后，回调函数（callback）就会触发。
#### 6.5.2. $.get()和$.post()方法
$.get()和$.post()是jQuery中的**全局函数**；在此之前的方法都是对jQuery对象进行的操作；
1. $.get()方法

结构：**$.get(url[, data][, callback][, type])**
url：请求的HTML页的URL地址；
data：Object，发至服务器的key/value值会作为QueryString附加到请求url中；
callback：Function，**载入成功**时回调函数，回调函数有两个参数data和textStatus；
type：服务器端返回内容的格式，包括xml，html，script，json，text，_default；
````html
<form id="form1" action="#">
    <p>评论: </p>
    <p>姓名: <input type="text" name="username" id="username" /></p>
    <p>内容: <textarea name="content" id="content" cols="20" cols="2" ></textarea></p>
<p><input type="button" value="提交" id="send" /></p>
    <div class="comment">已有评论：</div>
    <div id="resText"></div>
</form>
````
````javascript
$(function(){
    $("#send").click(function(){
//      $.get(url , data , callback , type)
        $.get("get1.php" , {
            username: $("#username").val(),
            content: $("#content").val()
        } , function(data, textStatus){
//data: 返回的内容，可以是HTML片段、XML文档、JSON文件等等
//testStatus: 请求状态：success, error, notmodified, timeout
            $("#resText").html(data);
        })
    })
})
````

2. $.post()方法

$.post()和$.get()的区别：
- GET请求会**将参数跟在URL后**进行传递，为POST请求则是作为**HTTP消息的实体内容**发给web服务器；区别在于对用于是否可见；
- GET请求对传输数据的大小有限制（<2kb）；POST理论上没有限制；
- GET方式请求的数据会被浏览器缓存起来，有安全问题；
- 在服务器端的获取也不同。在PHP中，$_GET[]和$_POST()；

结构和使用方式与$.get()相同: **$.post(url , data , callback , type)**
````javascript
$(function(){
    $("#send").click(function(){
//      $.post(url , data , callback , type)
        $.post("post1.php" , {
            username: $("#username").val(),
            content: $("#content").val()
        } , function(data, textStatus){
            $("#resText").html(data);
        })
    })
})
````
#### 6.5.3. $.getScript()和$.getJson()方法
1. $.getScript()方法

使用$.getScript()加载js文件: **$.getScript("test.js" , callback)**，回调函数在js成功载入后执行；
````javascript
$(function(){
    $.getScript("jquery.color.js" , function(){
        $("#go").click(function(){
            $(".block").animate({backgroundColor: "pink"}, 1000)
        })
    })
})
````
2. $.getJSON()方法

$.getJSON()用于加载JSON文件:**$.getJSON(url , callback)**，请求成功后调用回调函数
````javascript
$(function(){
    $.getJSON("test.json" , function(data){
//      data: 返回的数据
        var html="";
        $.each(data, function(commentIndex, comment){
//          commentIndex: 对象的成员或者数字的索引
//          comment: 对应变量或内容
            html+="<p>"+comment.username+": </p><p>"+comment.content+"</p>"
        })
        $("#resText").html(html);
    })
})
````
上面的代码中用到一个$.each()函数，这个区别于jQuery对象中的each()方法，它是一个全局函数，不操作jQuery对象；
**$.each(data, function(index, content){})**
callback中的两个参数：**对象的成员或者数组的索引、对应变量的内容**
data是**要遍历的数组或者对象**；
> JSONP跨域访问：page188。
#### 6.5.4. $.ajax()方法
$.ajax()是jQuery最底层的AJAX实现：**$.ajax(options)**
option是一个对象，包括了这个方法所需要的请求设置和回调函数等信息，所有的参数都是可选的；

|参数名称|类型|说明|
|:-------:|:---:|:---|
|url|String|发送请求的地址|
|type|String|请求方式POST或GET，默认为GET|
|timeout|Number|设置请求超时时间|
|data|Object<br />/String|发送到服务器的数据<br />如果已经不是字符串，则自动转换为字符串，对象必须是{key:value}格式|
|dataType|String|预期服务器返回的数据类型；<br />xml:返回xml文档，可用jQuery处理；<br />html:返回纯文本html信息；包含的script标签会在插入DOM时执行；<br />script:返回纯文本JS代码；<br />json:<br />jsonp:<br />text:返回纯文本字符串；|
|beforeSend|Function|
|complete|Function|
|success|Function|
|error|Function|
|global|Boolean|

eg1：$.getScript()的$.ajax()实现
````javascript
$(function(){
    $("#send").click(function(){
        $.ajax({
            type: "GET",
            url: "test.js",
            dataType: "script"
        })
    })
})
````
eg2 上面$.getJSON()的$.ajax()实现
````javascript
$.ajax({
    type: "GET",
    dataType: "json",
    url: "test.json",
    success: function(data){
        $.each(data, function(index, content){
//          获取content中的内容并操作
        })
    }
})
````
