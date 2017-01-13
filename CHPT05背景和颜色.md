CHPT5--背景和颜色
===
### 5.1 设定背景图的大小
---
CSS3种通过background-size的属性来设置：
````css
div{
    background: url(img/img1.gif);
    background-size: 80px 60px;    /*宽度x高度*/
    background-repeat: no-repeat;
}
div{
    background-size: 100% auto;    /*相当于属性为contain*/
}
````
background-size还有两个可选项：cover和contain，contain相当于宽度设置为元素宽度、高度为auto；而cover则相当于高度设置为元素高度、宽度为auto；
### 5.2 利用图片叠加实现多背景
---
层的上下按照在CSS中书写的顺序来定，最先写的背景在最上面，每层图片的定义用逗号隔开；
````css
div{
    background: url() 0 0 no-reprat,
                url() 200px 0 no-repeat;
}
````
### 使用图片背景的origin和clip属性
---
#### 5.3.1 background-origin属性
指定背景图片的起始位置。正常情况下，背景图片是从内边距的左上角开始的（padding-box）
````css
div{
    background-origin: border-box;     /*边框左上角*/
    background-origin: padding-box;    /*内边距的左上角*/
    background-origin: content-box;    /*内容左上角*/
}
````
#### 5.3.2 background-clip属性
属性值同上，但是背景图片的大小不会变化，会控制元素背景的显示范围，也就是外围会被割去/增加。
### 5.4 颜色模式
---
RGBA格式：原RGB格式增加一个Alpha透明度；
HSLA模式：
````css
div{
    background: hsla(Hue, Saturation, Light, Alpha);
/*  hue：色调
    saturation：饱和度
    light：亮度
    alpha：透明度
}
````
### 5.5 透明颜色
---
opacity属性：
````css
.image{
    fliter: Alpha(Opacity=20);    /*IE兼容代码，应用IE中的滤镜效果*/
    opacity: 0.2
}
````
### 5.7 渐变
---
渐变可以分为linear-gradient线性渐变和radial-gradient放射渐变
#### 5.7.1 线性渐变linear-gradient
可以指定渐变的方向：
````css
.test{
    background: -webkit-linear-gradient(left, red, blue);  /*webkit核心浏览器兼容代码*/
    background: -o-linear-gradient(left, red, blue);  /*opera浏览器兼容代码*/
    background: -moz-linear-gradient(left, red, blue);  /*firefox浏览器兼容代码*/
    background: linear-gradient(to right, red, blue);  /*标准写法*/
}
````
当然颜色可以设置不止两种，只需要用逗号隔开就行

#### 5.7.2 放射渐变
radial-gradient(position, color 5%, color2 10%,...)
### 5.8 一些实例
---
#### 5.8.1 带立体突起效果的按钮
````css
.grey-dropdown{
    background: -webkit-linear-gradient(#ffffff, #e9e9e9);
    background: -moz-linear-gradient(#ffffff, #e9e9e9);
    background: -o-linear-gradient(#ffffff, #e9e9e9);
    background: linear-gradient(#ffffff, #e9e9e9);
}
````
#### 5.8.2 构造尺寸更灵活的背景
采用CSS方案替换图片，如果标题折行，则自动撑大标题区域。不管几行都可以完美适配。
````css
.header{
    background-image: -webkit-linear-gradient(left, #241a38, #012c57, #031a40);
    background-image: -o-linear-gradient(left, #241a38, #012c57, #031a40);
    background-image: -moz-linear-gradient(left, #241a38, #012c57, #031a40);
    background-image: linear-gradient(left, #241a38, #012c57, #031a40);
}
````
#### 5.8.3 使用放射渐变制作光影效果
````css
.box{
    width: 200px;
    height: 200px;
    font-size: 80px;
    line-height: 200px;
    text-align: center;
    background: -webkit-radial-gradient(#feb3ad. #fd695d);
    background: -o-radial-gradient(#feb3ad. #fd695d);
    background: -moz-radial-gradient(#feb3ad. #fd695d);
    background: radial-gradient(#feb3ad. #fd695d);
}
````