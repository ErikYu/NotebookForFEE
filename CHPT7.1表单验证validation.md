jQuery插件的使用和写法
===
### 7.1 表单验证validation
---
#### 7.1.1-7.1.3 一个简单的表单验证案例
1. 步骤

- 引入jQuery库和Validation插件
- 确定哪个表单需要验证，**$("#commentForm").validate()**
- 针对不同的字段，进行验证规则编码，设置字段的相应属性
  - class="required"为必须填写，minlength="2"为最小长度为2；
  - class="required email"为必须填写且需要符合Email格式；
  - class="url"为url格式验证
````html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <script src="js/jquery-3.1.0.js"></script>
        <script src="js/jquery.validate.js"></script>
        <style type="text/css">
            *{
                font-family: verdana;
                font-size: 96%;
            }
            label{
                width: 10em;
                float: left;
            }
            label.error{
                float: none;
                color: red;
                padding-left: 0.5em;
                vertical-align: top;
            }
            p{
                clear: both;
            }
            .submit{
                margin-left: 12em;
            }
            em{
                font-weight: bold;
                padding-right: 1em;
                vertical-align: top;
            }
			
        </style>
        <script>
            $(function(){
                $("#commentForm").validate()
            })
        </script>
    </head>
    <body>
        <form class="cmxform" id="commentForm" method="get" action="#">
			<fieldset>
                <legend>一个简单的带验证提示的评论例子</legend>
                <p>
                    <label for="cusername">姓名</label><em>*</em>
                    <input id="cusername" name="username" size="25" class="required" minlength="2" />
                </p>
                <p>
                    <label for="ceamil">电子邮件</label><em>*</em>
                    <input id="cemail" name="eamil" size="25" class="required email" />
                </p>
                <p>
                    <label for="curl">网址</label><em>&nbsp;</em>
                    <input id="curl" name="url" size="25" class="url" value="" />
                </p>
                <p>
                    <label for="ccomment">你的评论</label><em>*</em>
                    <textarea id="ccomment" name="comment" cols="22" class="required"></textarea>
                </p>
                <p>
                    <input class="submit" type="submit" value="提交" />
                </p>
            </fieldset>
        </form>
    </body>
</html>

````

#### 7.1.4 不同的验证写法
将验证相关的信息全部写进class属性里面，需要的步骤如下：
- 引入一个新的jQuery插件，jquery.metadata.js；
- 改变调用的验证方法：**$("#commentForm").validate({meta: "validate"})**；
- 将验证规则全部写进class属性中；**class="{validate:{required: true, minlength:2}}"**
````html
<p>
    <label for="cusername">姓名</label><em>*</em>
    <input id="cusername" name="username" size="25" class="{validate:{required: true, minlength: 2}}" />
</p>
<p>
    <label for="ceamil">电子邮件</label><em>*</em>
    <input id="cemail" name="eamil" size="25" class="{validate: {required: true, email: true}}" />
</p>
<p>
    <label for="curl">网址</label><em>&nbsp;</em>
    <input id="curl" name="url" size="25" class="{validate:{url: true}}" value="" />
</p>
<p>
    <label for="ccomment">你的评论</label><em>*</em>
    <textarea id="ccomment" name="comment" cols="22" class="{validate:{required: true}}"></textarea>
</p>
<p>
    <input class="submit" type="submit" value="提交" />
</p>
````


以上验证规则都是通过设置一定的属性值实现的，但是验证行为和HTML并没有完全脱钩。下例中需要实现结构与行为的分离。
- 将字段中的class属性移除；
- 在$("#commentForm").validate()中添加rules属性；
- 通过每个字段的**name属性值**来匹配验证规则；
- 定义验证规则

````javascript
$(function(){
    $("#commentForm").validate({
        rules: {
            username: {
                required: true,
                minlength: 2
            },
            email: {
                required: true,
                eamil: true
            },
            url: {
                url: true,
            },
            comment: "required"    //单属性可写作此
        },
        messages:{
            username: {
                required: "必须填写用户名",
                minlength: "最小2位"
            },
            email: {
                required: "必须填写邮箱",
                email: "必须是邮箱格式"
            }
        }
    })
})
````
#### 7.1.5 验证信息

1. 引入语言库

2. 自定义验证信息

#### 7.1.6 自定义验证规则
下例为增加一个验证码验证的代码：
首先自定义一个验证规则：
````javascript
$.validator.addMethod(
    "fomula",    //验证方法的名称
    function(value, element, param){
        return value=eval(param);  //value是值，eval()计算字符串运算的值
    },
    "请输入正确的值"    //验证提示信息
)
````
然后调用该规则：
````javascript
$("#commentForm").validate({
    rules: {
        username: {required: true, minlength: 2},
        email: {required: true, email: true},
        url: "url",
        comment: "required",
        valcode: {formula: "7+9"}   //valcode是是需要验证的元素的name值，eval()是计算字符串的方法
    }
})
````
#### 7.1.7 validation的API
> 见附录F
> 以下为mooc有关validation plugin内容
##### 7.1.7.1基本API
1. 介绍两个重要的概念

- method：验证方法，值得是检验的逻辑。比如email方法，就是一个method，检验输入的文本是否符合email的规则；
- rule：验证规则，指的是元素和验证方法的关联。

2. validate()方法，包括了验证规则和其他一些配置项，上面的调用验证规则就是通过对表单元素调用validate()方法完成的。

````javascript
$(elemForm).validate({debug: true, rule: {}, messages: {}})
````

3. 基本验证方法

- required 必填 
- remote 远程校验

发送GET请求，值为一个处理的URL，通过返回的true/false判断是否验证通过
发送POST请求，例子如下 
````javascript
$(formElement).validate({
    rules:{
        required: true,
        remote: {
            url: "remote.json",
            type: "post",
            data:{
                loginTime: function(){
                    return +new Date();
                }
            }
        }
    }
})
````

- minlehgth最小长度  maxlength最大长度
- rangelength长度范围，值是一个数组，包括最小值和最大值
- min max range数字范围
- email url date日期格式（2011-11-11T11:11:11）(2011/11/11)等等都有效
- dateISO：标准日期校验  yyyy-mm-dd  yyyy/mm/dd
- number数字 digits非负整数 
- equalTo与另一个元素值相等，经常用于确认密码框

````javascript
$(function(){
    validator=$("#loginForm").validate({
        debug: true,
        rules: {
            username: {
                required: true,
                minlength: 2,
                maxlength: 10
            },
            password:{
                required: true,
                minlength: 6,
                maxlength: 16
            },
            conPassword:{
                // required: true,
                equalTo: "#password"    //这里是个id选择器
            },
            email: {
                email: true
            }
        }, 
        messages: {
            username: {
                required: "这里要有字",
                minlength: "不能少于两个字",
                maxlength: "不能多于十个字"
            },
            password:{
                required: "密码还是要的",
                minlength: "太短了",
                maxlength: "太长了"
            },
            conPassword:{
                // required: true,
                equalTo: "两个密码需要相同"
            },
            email: {
                email: "邮件格式"
            }
        }
    })
})
````

##### 7.1.7.2 高级API

1. valid()方法：对一个表单（整个表单）元素调用valid()方法，返回true和false，检查表单是否填写正确，即表单内容是否有效/是否满足验证规则；
2. rules()方法：

- rules()：获取表单里的**某个元素**的校验规则；
- rules("add", rules)：向表单元素增加校验规则；
`$("#username").rules("add", {minlength: 2, maxlength: 10})`
- rules("remove", rules)
`$("#username").rules("remove", "maxlength minlength");`

3. validator对象：validate()方法返回一个Validator对象，可以将他赋值给一个变量。可以对这个变量调用以下方法：
- Validator.form(): 返回true/false，描述表单是否填写正确；
- Validator.element( element ): 返回true/false，描述表单中某个元素是否有效；
- Validator.resetForm(): 将表单恢复到验证之前的原来的状态（错误信息被清除掉）；
- Validator.showErrors( error ): 增加显示其他错误信息
- Validator.numberOfInvalids(): 返回无效的元素数量
````console
validator.form()                //返回true/false，描述表单是否填写正确
validator.element("#password")  //返回true/false，描述表单中某个元素是否有效；
validator.resetForm()           //恢复到验证之前
validator.showErrors({          //针对某个元素显示特定的错误信息
  username: "你输错了",
  password: "密码错误"
})
validator.numberOfInvalids()    //返回无效的元素数量
````

4. validator对象的静态方法，可以直接调用

`jQuery.validator.format()`: 格式化字符串，用参数替代模板中的{n};
````javascript
var template=$.validator.format("{0}-{1}-{2}");
template("1","2","3");   //"1-2-3"
````
`jQuery.validator.setDefaults( options )`: 修改**插件默认设置**，例如给页面上的所有的十个表单增加一个debug: true
````javascript
$.validator.setDefaults({
  debug: true
})
````
`jQuery.validator.addClassRules( name, rules )`: 为某些包含名为name的class增加组合验证类型
````javascript
$.validator.addClassRules("txt", {            //txt是class属性的名称
  required: true
})
````
##### 7.1.7.3 validate()方法配置项

1. submitHandler: 通过验证后运行的函数，可以加上表单提交的方法；传入一个参数form，`submitHandler: function(form){ //do something }`
2. invalidHandler: 无效表单提交后运行的函数，传入两个参数event和validator；`invalidHandler: function(event, validator){}`
3. ignore: 对某些元素不进行校验。默认是`ignore: "hidden"`，值是一个选择器，对符合条件的元素忽略validate;
4. rules: 定义校验规则；rules中的每一个验证方法都会定一个depends属性，这个属性是true的时候才会进行这项验证，例如，只有当密码被填写时才会进行用户名必填验证；
````javascript
$("#loginForm").validate(
    rules: {
        username: {                  //name属性
            required: {              //本身值是true/false，不需要传入参数param
                depends: function(element){
                    return $("#password").is(":filled");   //filled是validation插件自带的一个选择器，下面会降到的
                }
            },
            minlength: {
                param: 2,            //传入参数
                depends: function(element){         //返回true才会执行验证
                    return $("#password").is(":filled");
                }
            }
        },            
        password: {}
    }, 
    messages{})
````
5. messages: 定义提示信息；
6. groups: 用一个错误提示显示对一组元素的验证；通常跟errorPlacement一起使用，把错误信息放在某个位置。
7. onsubmit: 是否在提交时验证，默认为true
8. onfoucsout: 是否在获取焦点时验证
9. onkeyup: 是否在敲击键盘时验证
10. onclick: 是否在鼠标点击时验证，一般用于checkbox或者radio
11. focusInvalid: 提交表单后，未通过验证的表单是否获得焦点；默认为true；
12. focusCleanup: 未通过验证的元素获得焦点时，是否移除错误提示；默认为
13. errorClass: 指定错误提示的css类名，可以自定义错误提示的样式；
14. validClass: 指定验证通过的css类名
15. errorElement: 使用什么标签标记错误，默认是label标签；`errorElement: "b",  //使用<b></b>标签`
16. warpper: 使用什么标签把上面的errorElemnt包裹起来；
17. errorLabelContainer: 把错误信息统一放在一个容器中；
18. errorContainer: 显示或者隐藏验证信息，可以实现有错误信息出现时把容器属性变为显示，无错误时隐藏；
````javascript
$().validate({
    rules:{},
    messages:{},
    errerElement: "li",             //用<li>标签显示错误信息
    wrapper:"ul",                   //用<ul>标签包裹错误信息
    errorLabelContainer: "#info",   //在#info中集中显示错误信息
    errorContainer: "#info2",       //表单有错误时#info2显示，无错误时隐藏
})
````
19. showErrors: 可以显示总共有多少个未通过验证的元素；`showErrors: function(errorMap, errorList){}`
20. errorPlacement: 自定义错误信息放在哪个位置；
21. success: 要验证的元素通过验证后的动作，针对验证标签元素；`success: "right",  //给显示错误信息的label增加一个class`
22. highlight: 可以给未通过验证的元素加效果，针对表单元素；
23. unhighlight: 去除未通过验证的元素效果，一般和highlight同时使用；
````jaavascript
$().validate({
    highlight: function(element, errorClass, validClass){
        $(element).fadeOut().fadeIn();
    }
})
````
jQuery Validation插件有几个扩展的选择器：
:blank 选择所有值为空的元素；
:filled 选择所有值不为空的元素；
:unchecked 选择所有没被选中的元素；
##### 7.1.7.4 自定义验证方法
````javascript
jQuery.validator.addMethod( name, method [, message])
//name: 方法名称
//method: function(value, element, params) 方法逻辑
message: 提示消息
````
````javascript
$.validator.addMethod("postcode",                //验证方法的名字 
function(value,element, params){                 //value是表单元素的value，element是表单元素，param是验证方法的值
    var postcode=/^[0-9]{6}$/;
    return this.optional(element) || postcode.test(value);
    //第一个表达式是指元素为空时不进行这项验证
    //第二个表达式是传入的value进行postcode验证
}, 
$.validator.format("请输入正确的{0}邮编"))
````

### 7.2表单插件--Form
---
#### 7.2.1-7.2.3 简介
jQuery Form插件是一个Ajax表单插件，两个核心方法是ajaxForm()和ajaxSubmit()，他们集合了从控制表单元素到决定如何管理提交进程的功能。
````html
<head>
<script src="js/jQuery.js"></script>
<script src="js/jquery.form.js"></script>  <!--引入form插件-->
<script type="text/javascript">
    $(function(){
        $("#myForm").ajaxForm(function(){
            $("#output1").html("提交成功，欢迎下次再来！").show();
        });
    });
</script>
</head>
<body>
    <form id="myForm" action="demo.php" method="post">
        名称：<input type="text" name="name" /><br />
        地址：<input type="text" name="address" /><br />
        自我介绍：<textarea name="content"></textarea><br />
        <input type="submit" id="test" value="提交" /><br />
        <div id="output1" style="display: none"></div>
    </form>
</body>
````
#### 7.2.4 核心方法-ajaxForm()和ajaxSubmit()

1. ajaxForm()方法，将表单升级为ajax提交方式；

````javascript
$("#myForm").ajaxForm(function(){
    $("#output1").html("Success!");
});
````

2. ajaxSubmit()方法，该方法响应用户的提交表单操作，从而使表单的提交方式升级为ajax提交方式；

````javascript
$("#myForm").submit(function(){
    $(this).ajaxSubmit(function(){
        $("#output1").html("success!");
    });
    return false;  //阻止表单默认提交
});
````
#### 7.2.5 ajaxForm()和ajaxSubmit()方法的参数
　　ajaxForm()和ajaxSubmit()都可以接收一个或零个参数，一个参数可以是回调函数，也可以是对象options。
````javascript
var options={
    target: "#output1",    //把服务器返回的内容放入id为output1的元素中
    beforeSubmit: showRequest,  //提交前的回调函数
    success: showResponse,      //提交后的回调函数
    url: url,              //默认是form的action，如果申明，则覆盖
    type: type,            //默认是form的method
    dataType: null,        //接收服务端返回的内容，"script" "json" "xml"
    clearForm: true,       //成功提交后，清除所有表单元素的值
    resetForm: true,       //成功提交后，重置所有表单元素的值
    timeOut: 3000          //限制请求的时间，单位ms
}

$("#myForm").ajaxForm(options);   //把options对象传递给ajaxForm()方法

$("#myForm").submit(function(){   //或者传递给ajaxSubmit()方法
    $(this).ajaxSubmit(options);
    return false;    //阻止表单默认提交
});
````

在options对象中有两个回调函数
- beforeSubmit-----提交前的回调函数；
这个回调函数有三个参数：
````javascript
function showRequest(formData, jqForm, options){
//  formData: 数组对象
//  jqForm: jQuery对象，封装了表单的元素
//  options: options对象
    var quertString=$.param(formData);  //将数组对象转化为字符串
    return true;
}
````
- success-----提交后的回调函数
有四个参数：
````javascript
function showResponse(responseText, statusText, xhr, $form){
//  responseText: 携带服务器返回的数据，会根据options对象中dataType属性来返回相应格式的内容
//  statusText: 返回状态，如success或error
}
````
responseText返回的内容具体如下：
> 1) 缺省的HTML返回，该参数是XMLHttpRequset对象的ResponseText属性；
> 2) xml返回，该参数是XMLHttpRequset对象的responseXML属性；
> ````javascript
> function processXML(responseXML){
>     
> }
> ````