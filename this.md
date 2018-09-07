# JavaScript中的this

## this是什么

在js中this非常常见，但是要完全理解this关键字的原理以及在代码中如何使用对相当一部分的开发者来说着实不易。

今天就由来给大家详细的介绍一下js中的this；

// js中this关键字是包含它的函数作为方法被调用时所属的对象。

// 非严格模式下的情况
// 全局环境中的this
// 函数中的this
// 方法中的this
// 事件中的函数中的this
// 构造函数中this
// call和apply
// bind
// 箭头函数中的this

// 严格模式下的一些情况

首先我们用文字来定义什么是对象呢：



js中的this关键字是包含它的函数作为方法被调用时所属的对象。

看似很抽象的一句话，但实际理解起来比较困难，我们可以分3部分来理解它！
1、包含它的函数。2、作为方法被调用时。3、所属的对象。

首先我们先来看一下js中非严格模式下的this分别都是什么

## 全局环境中的this

在全局执行上下文中（在任何函数体外部）this 都指代全局对象。
	
	// 在浏览器中, window 对象同时也是全局对象：
	console.log(this === window); // true
	
	a = 37;
	console.log(window.a); // 37
	
	this.b = "MDN";
	console.log(window.b)  // "MDN"
	console.log(b)         // "MDN"


## 函数中的this

绝大多数情况下，函数的调用方式决定了this的值。

在严格模式和非严格模式之间也会有一些差别。

	function f1(){
	  return this;
	}
	//在浏览器中：
	f1() === window;   //在浏览器中，全局对象是window

然而，在严格模式下，this将保持他进入执行上下文时的值，所以下面的this将会默认为undefined。

	function f2(){
	  "use strict"; // 这里是严格模式
	  return this;
	}
	
	f2() === undefined; // true

自执行函数执行，方法中的this一般都是window

	var obj = {
		fn: (function() {
			// this -> window
		})()
	};

	~function() {
		// this -> window
	}()

## 方法中的this

我们说定义在对象内的函数叫做方法，而这个方法执行的时候，它的this就是它的调用者

	var obj = {
		fn: function () {
			console.log(this);
		}
	};

	obj.fn();

此时fn方法的调用者为obj，所以fn中的this就是obj对象；

但我们把这个方法赋值给一个自由变量的时候

	var f1 = obj.fn;

	f1();

此时f1执行，实际是obj.fn执行，但f1的调用者实际上是  window,  window.f1()  所以fn中的this是window

下面再看一个例子，
我们调用数组上的slice方法，

	[].slice(); // this -> []
	
	[].__proto__.slice(); // this -> [].__proto__
	
	Array.prototype.slice();  // this -> Array.prototype

三种调用的slice方法是一样的，但调用的方式不同，里面的this也不同

## 事件中的函数中的this

给元素的某个事件绑定了一个方法，当事件触发，函数执行的时候，绑定的这个方法中的this一般是当前操作的这个DOM元素

'二般'情况下

IE6~8下，如果我们使用DOM2事件绑定，方法执行的时候，里面的this不是当前元素，而是window

	ele.attachEvent('onclick', function() {
		console.log(this);  // window
	});

## 构造函数中的this

构造函数执行的时候，函数体中的this都是当前实例；

	function Fn() {
		// this -> 当前Fn的实例
		// this.name 是给当前实例设置私有属性
		this.name = '1000phone'
	}

	var f = new Fn();

## call和apply

当一个函数在其主体中使用 this 关键字时，可以通过使用函数继承自Function.prototype 的 call 或 apply 方法将 this 值绑定到调用中的特定对象。

	var school = {name: '1000phone'};
	
	function sum(num1, num2) {
		this.total = num1 + num2;
	}

	sum(20, 30); // this -> window

	sum.call(school, 20, 30);  

首先让 sum 中的this指向call方法中的第一个参数，然后执行sum这个函数

此时 sum 中的 this -> school   num1 -> 20   num2 -> 30

参数位置是固定的，如果第一个参数不是 school

	sum.call(20, 30);

这时候 sum 的this 是 20，num1 -> 30   num2 -> undefined

如果我们一个参数都不传

	sum.call();

sum 的 this -> window   num1 = num2 = undefined

相当于 sum 直接执行，不传参

	sum();

但如果第一个参数为null或者undefined
那么就代表没有this，函数中的this依然是window

apply语法与作用，和call类似，只有一个区别，apply传入一组参数

	sum.call(school, 20, 30);

	sum.apply(school, [20, 30]);

apply的语法要求写成一个数组，但实际和call一样，也是一项一项的给形参赋值的

	~function () {
		console.log(this);
	}.call(school);

此时 自执行函数中的this被call指向为 school

	Array.prototype.slice.call(arguments);

此时slice方法执行的时候中的this将不再是 Array.prototype，而是arguments

call和apply之前的规则，遇到call/apply时，都将以用户自主指向的this为主

## bind

ECMAScript 5 引入了 Function.prototype.bind。调用f.bind(someObject)会创建一个与f具有相同函数体和作用域的函数，但是在这个新函数中，this将永久地被绑定到了bind的第一个参数，无论这个函数是如何被调用的。

	var school = {name: '1000phone'};
	
	function sum(num1, num2) {
		this.total = num1 + num2;
	}

	sum.call(school, 20, 30); 

this -> school 并且执行sum

	sum.bind(school, 20, 30); 

虽然改变了this指向，但并没有执行sum，它是预先处理this和实参，不会立即执行，

通常我们让它在某个特定条件，才会被触发   IE6~8不兼容

例如我们有个需求，一秒后执行sum，此时需要定时器，执行sum时，sum中this指向school，并且传参。

	setTimeout(sum, 1000);

我们一秒后执行sum函数

但此时sum中的this是window

	setTimeout(sum.call(school, 20, 30), 1000);

此时实现了this的指向并且传了参数，但sum立即执行了，不是一秒后

	setTimeout(function() {
		sum.call(school, 20, 30);
	}, 1000);

我们可以达到一秒后执行sum，this指向为shool，并且传参，执行，但代码并不理想，我们可以使用bind预处理能够达到我们的目的

	setTimeout(sum.bind(school, 20, 30), 1000);

## 箭头函数中的this

	var obj = {
		fn: function() {

			setTimeout(function() {
				// this -> window
			}, 1000);

			var that = this;
			setTimeout(function() {
				// that -> obj
			}, 1000);

			setTimeout(function() {
				// this -> obj
			}.bind(this), 1000);

			setTimeout(() => {
				// this -> obj
			}, 1000);
		}
	};

在箭头函数中，this与封闭词法上下文的this保持一致，也就是说箭头函数中继承了词法作用域的this

	
## 严格模式下

非严格模式下，不明确执行主体，浏览器认为执行主体默认windo  this一般都是window

严格模式下，执行主体不明确，this是undefined

	'use strict'
	
	~function(){
		// this -> undefined
	}();

	~function(){
		
		// this -> undefined
	}();

	fn() // undefined

	window.fn()  // window

	fn.call() // undefined

	fn.call(window) // window

	fn.call(null) // null

	fn.call(undefined) // undefined

apply同call
	



可以理解为含有this的函数的调用者。并且this和函数在哪执行的，以及定义的位置没有直接关系