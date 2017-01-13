CHPT6--jQuery和Ajax应用
===
### 6.6 序列化元素
---
1. serialize()方法

serialize()方法作用于一个jQuery对象，能够将DOM元素内容序列化为字符串;
````javascript
$.get("get1.php", $("#form1").serialize(), function(data, textStatus){
    $("#resText").html(data);
});
````
将原对象改为serialize()方法后，即使在表单中添加新的元素，脚本仍然能够使用。
serialize()方法作用域jQuery对象，其他选择器选取的元素都能使用，如：$(":checkbox,:radio").serialize();

2. serializeArray()方法

与serialize()类似，但是不是返回字符串，而是将DOM元素序列化之后，返回JSON格式的数据。
````javascript
$(function(){
    var fields=$(":checkbax,:radio").serializeArray();
    console.log(fields);
    $.each(fields, function(index, content){
        $("#result").append(fields.value+", ")
    })
})
````

3. $.param()方法

serialize()方法的核心，用来对一个数组或对象按照key/value进行序列化;
````javascript
var obj={a:1, b:2, c:3};
var k=$.param(obj);
console.log(k);//a=1&b=2&c=3
````
### 6.7 jQuery中的Ajax全局事件
---
$(elem).ajaxStart(function(){})：ajax请求开始执行
$(elem).ajaxStop(function(){})：ajax请求结束执行
$(elem).ajaxComplete(function(){})：ajax请求完成执行
$(elem).ajaxError(function(){})：ajax请求发生错误执行
$(elem).ajaxSend(callback)：ajax请求发送前执行
$(elem).ajaxSuccess(callback)：ajax请求成功时执行

> 在jQuery1.5之后，如果Ajax不触发全局方法，需要设置
> ````javascript
> $.ajaxPrefilter(function(options){
>     options.global=true;
> })
> ````
> 这里最新的jQuert3.1未实现，原因未知。
### 6.8 基于jQuery的Ajax聊天室
---
