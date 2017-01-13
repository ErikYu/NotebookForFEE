CHPT6--更个性的边框
===
CSS3新增了三种边框属性：
- border-radius：圆角边框
- border-shadow：边框阴影
- border-image：图片边框

### 6.1 圆角边框
---
最基本的用法：设置四个相同弧度的圆角：
````css
div{
    border: 1px solid;
    border-radius: 5px;
    -moz-border-radius: 5px;    /*支持旧版firefox*/
    -webkit-border-radius: 5px; /*支持旧版webkit核心浏览器*/
}
````
可以使用百分比作为单位：根据边的长度百分比进行调整；
设置不同弧度的圆角：
````css
div{
    border: 1px solid;
    border-top-left-radius: 5px;
    border-top-right-radius: 5px;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
}
````
### 6.2 边框阴影
---
严格来说，box-shadow跟边框并没有什么关系，即使不设置边框也可以设置box-shadow。
````css
div{
    box-shadow: color h-shadow v-shadow blur spread color inset;
/*  color: 可选，阴影颜色
    h-shadow: 必选，水平阴影的位置，允许负值
    v-shadow: 必选，垂直阴影的位置，允许负值
    blur: 可选，模糊距离
    spread: 可选，阴影的尺寸
    inset: 可选，外阴影改为内阴影
*/
}
````
和边框不同，阴影是不占据空间的
### 6.3 图片边框
---
连IE10都不支持的渣渣，opera需要-o-支持
````css
div{
    border-image-source: url();
    border-image-slice: [ number | percentage]; /*number只是数字，不可以加单位，默认单位是px*/
    border-image-width: number;    /*规定了图片边框的宽度*/
    border-image-outset: number;    /*规定了图片边框外延的距离，默认是0，即图片边框在元素内部*/
    border-image-repeat: repeat/round/sketch;
}
````
### 6.4 通过resize属性来改变输入框的大小
---
目前只有webkit核心的浏览器才支持这个属性；
````css
textarea{
    resize: none;    /*用户无法调整元素尺寸*/
    resize: both;    /*用户可以同时调整元素宽高*/
    resize: horizontal;
    resize: vertical;
}
````