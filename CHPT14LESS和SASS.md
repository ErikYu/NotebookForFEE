CHPT14--LESS和SASS
===
### 14.1 CSS的缺陷
---
- 无法定义变量；
- 重复代码
- 计算问题
- 作用域和命名空间
### 14.2 LESS
---
LESS使用：引入less文件，再引入less.js
引入操作
`<link rel="stylesheet/less" type="text/css" href="css/test.less"/>`
#### 14.2.3 使用变量和操作符
````less
/*LESS代码*/
@color: #4d926f;
#header{
    color: @color;
}
h2{
    color: @color;
}
````
#### 14.2.4使用Mixin混入
````LESS
.rounded-corners(@radius: 5px){
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    -o-border-radius: @radius;
    -ms-border-radius: @radius;
    border-radius: @radius;
}
#header{
    .rounded-corners;  /*. 不能省略*/
}
#footer{
    .rounded-corners(10px); /*单独修改*/
}
````
Mixin的语法关键在.符号
#### 14.2.5 内嵌规则
LESS可以使用嵌套的样式编写层叠样式表：
````less
#main{
    div li{
        list-style: none;
    }
    .container{
        margin: auto;
        width: 960px;
    }
    a{
        text-decoration: none;
    }
}
````
#### 14.2.6 运算
任何数字颜色变量都可以参加运算
````css
@base: 5%;
@filter: @base*2;
@other: @base+@filter;
color: #888 / 4;
background-color: @base-color + #111;
height: 100%/2+@filter;
````
LESS的运算能够分辨出颜色和单位，
`@length: 1px+7; /*8px*/`
可以在复合属性中进行运算：
`border: (@width*2) solid black; `
### 14.3 SASS
---
