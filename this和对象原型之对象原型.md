this和对象原型2#
===
### 对象

---
1. 简单基本类型本身不是对象（string, boolean, number, null, undefined）
2. 内置对象（String, Number, Boolean, Object, Function, Array, Date, RegExp, Error）
3. 数组
   数组也是对象，可以给数组添加属性，但是数组的length并不会发生变化；**但是如果这个属性看起来像一个数字，那么就会修改数组内容**，而不是增加一个属性
4. 属性描述符，通过defineProperty来添加：
   - writable：决定是否可以修改属性的值，以下对于属性值的修改静默失败。如果在严格模式下，这个方法会出错，TypeError。
	```javascript
	var obj={};
	Object.defineProperty(obj, "a", {
		value: 2,
		writable: false,
		configurable: true,
		enumerable: true
	})
	console.log(obj.a);//2
	obj.a=34;
	console.log(obj.a);//2
	```
   - configurable：设置为false后，
     - 则不能修改属性描述符，但是可以将writable由true改为false，反之不行；不管是否处于严格模式，试图修改一个不可配置的属性描述符都会抛出TypeError错误
     - 禁止删除这个属性；删除后静默失败；
   - enumerable：设置为false后，这个属性就不会出现在枚举中。for...in...
5. getter和setter：当一个属性定义setter、getter或者两者都有时，这个属性会被定义为“访问描述符”，与数据描述符相对；
6. 下例中，不管是对象文字语法中get a(){}的，还是defineProperty()中的显示定义，两者都会在对象中创建一个没有值的属性，对此属性的访问会自动调用隐藏函数，它的返回值会被当作属性访问的返回值；
```javascript
var myObject={
	//给a定义一个getter
	get a(){
		return 2;
	}
}
Object.defineProperty(myObject, "b", {  //目标对象和属性名
	get: function(){this.a*2},//给b设置一个getter
	enumerable: true
})
console.log(myObject.a);
console.log(myObject.b);
```
7. 同时设置getter和setter
```javascript
var myObject={
	get a(){
		return this._a_;
	},
	set a(val){
		this._a_=val;
	},
}
myObject.a=7;
console.log(myObject.a);
```