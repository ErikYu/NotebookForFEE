this和对象原型
===
### 关于this

-----
1. 如果函数对象内部引用它自身，那只用this是不够的。一般来说需要通过一个指向函数对象的词法标识符（变量）来引用它；
2. this既不指向函数自身也不指向函数词法作用域；
3. this实际上是函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用；
4. this是运行时进行绑定的，并不是编写时绑定，this的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

### this全面解析

---
1. 调用位置：分析调用栈（就是为了达到当前执行位置所调用的所有函数），**调用位置就在当前正在执行的函数的前一个调用中**；
```javascript
function foo(){
  //当前调用栈是：foo
  //因此，当前调用位置是全局作用域
  console.log("foo");
  bar();
}
function bar(){
  //当前调用栈是：foo -> bar
  //因此，当前调用位置是foo中
  console.log("bar");
  baz();
}
function baz(){
  //当前调用栈是：foo -> bar -> baz
  //因此，当前调用位置是bar中
  comsole.log("baz");
  
}
foo();//foo的调用位置
```
2. 几个绑定规则
   1. **默认绑定**：独立函数调用。this指向全局对象
   2. **隐式绑定**：当函数引用有上下文对象时，隐式绑定规则会把函数调用中的this绑定到这个上下文对象。
		```javascript
		function foo(){
		  console.log(this.a);
		}
		var obj={
		  a: 2,
		  foo: foo
		}
		var a="hey gus";
		obj.foo();//2
		```
      	2.1 隐式丢失问题：被隐式绑定的函数会丢失绑定对象，此时会应用默认绑定。

		```javascript
		function foo(){
		  console.log(this.a);
		}
		var obj={
		  a: 2,
		  foo: foo
		}
		var a="hey gus";
		var bar=obj.foo;
		bar();//"hey gus"
		```
		传入回调函数时，参数传递也是一种隐式赋值。
		```javascript
		function foo(){
		  console.log(this.a);
		}
		function doFoo(fn){
		  fn();
		}
		var obj={
		  a: 2,
		  foo: foo
		}
		var a="hey gus";
		doFoo(obj.foo);//"hey gus"
		```

   3. **显式绑定**：使用call()和apply()方法。foo.call(obj)，调用foo时强制把它的this绑定到obj上。从this绑定的角度上说，call()和apply()是一样的。
		```javascript
		function foo(){
		  console.log(this.a);
		}
		var obj={
		  a: 2,
		  foo: foo
		}
		var a="hey gus";
		foo.call(obj);//"hey gus"
		```
		以上的显式绑定仍无法解决丢失绑定问题
		3.1 硬绑定的典型就是创建一个包裹函数，传入所有的参数并返回所有接收到的值；
		```javascript
		function foo(something){
		  console.log(this.a, something);
		  return this.a+something;
		}
		var obj={
		  a:2
		}
		var bar=function(){
		  return foo.apply(obj, arguments)
		}
		var b=bar(3);//2,3
		console.log(b);//5
		```

		ES5提供内置方法function.prototype.bind(),bind()会返回一个硬编码的新函数，他会把参数设置为this的上下文并调用原始函数。
		```javascript
		function foo(something){
		  console.log(this.a, something);
		  return this.a+something;
		}
		var obj={
		  a:2
		}
		var bar=foo.bind(obj);
		var b=bar(4);
		console.log(b);
		```
		3.2 API调用的上下文
   4. **new绑定**：在JavaScript中，构造函数只是一些用new操作符时被调用的函数。使用new来调用foo()时，会构造一个新对象并把它绑定到foo()调用中的this上。
		```javascript
		function foo(a){
		  this.a=a;
		}
		var bar=new foo(2);
		console.log(bar.a);//2	
		```

3. 优先级顺序：
	1. 由new调用？绑定到新创建的对象；
	2. 由call或者apply或者bind调用？绑定到指定的对象；
	3. 由上下文对象调用，绑定到那个上下文对象（注意隐式丢失）；
	4. 默认：在严格模式下绑定到undefined，否则绑定到全局对象。
