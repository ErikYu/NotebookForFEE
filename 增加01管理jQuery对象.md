jQuery对象管理
### 1 过滤 Filtering
---
|名称|说明|实例|
|:---|:---|:---|
|eq(index)|获取第N个元素|$("#p").eq(1)|
|filter(expr)|筛选出与表达式匹配的元素集合|`$().filter(".selected")`选取带有selected类的元素|
|filter(fn)|筛选出与指定函数返回值匹配的元素集合，这个函数内部将对每一个对象计算一次，正如$.each()|`$().filter(function(index){})`|
|is(expr)|用一个表达式来检查当前选择的元素集合，如果其中至少一个元素符合就返回true，否则为false|`$().is(":animated")`判断是否正在执行动画|
|map(callback)|将一组元素转化为其他数组|
|not(expr)|删除与制定表达式匹配的元素|
|slice(start,end)|选取一个匹配的子集|`$("p").slice(0,1)`选取第一个p元素|

https://www.shiyanlou.com/courses/running