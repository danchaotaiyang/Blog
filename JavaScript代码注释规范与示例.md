#JavaScript代码注释范例

> 做为一个有情怀的Coder，最近收集了一下JavaScript代码注释范例，希望能够帮助大家撸得一手妖媚而又放荡的Bug。

## 普通注释

### 单行注释

**使用 ```//``` 作为单行注释。**

**单行注释符后与注释内容保留一个空格。**

	//bad comments
	// good comments

单行注释需要在说明的代码之上另起一行，并且在注释前插入空行。

```
	function isType(content, expect) {

		// good
	    // Using Object.prototype.toString to judge data types.
	    let type = Object.prototype.toString.call(content).replace(/\[object\s|\]/g, '');
	
	    return type === expect;
	}
	// bad
	console.log(isType('hello', 'String'));	   // return true
```

带有 ```// FIXME:``` 或 ```// TODO:``` 的前缀的注释可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 ```FIXME -- need to figure this out``` 或者 ```TODO -- need to implement```。

**使用 ```// FIXME:``` 标注问题。**

	class Calculator {
		constructor() {
			// FIXME: shouldn't use a global here
			total = 0;
		}
	}

**使用 ```// TODO:``` 标注问题的解决方式。**

	// Support: IE, Opera, Webkit
	// TODO: identify versions
	// getElementById can match elements by name instead of ID
	if ( newContext && (elem = newContext.getElementById( m )) &&
		contains( context, elem ) &&
		elem.id === m ) {

		results.push( elem );
		return results;
	}

### 多行注释

**多行注释星号 ```*``` 对齐，并且注释内容不要写在起始符号 ```/**```与结束符号 ```*/``` 所在行。**

	// bad
	/* matches from matchExpr["CHILD"]
	  1 type (only|nth|...)
	   2 what (child|of-type)
	*/

	// good
	/* 
		matches from matchExpr["CHILD"]
		1 type (only|nth|...)
		2 what (child|of-type)
	 */

## 文件注释（多行注释）

**使用 ```/* ... */``` 作为文件注释。**

文件注释主要包含：概要介绍、版本信息、版权声明、开源协议、修改时间等说明内容。

*React.js*

	/** @license React v16.4.0
	 * react.development.js
	 *
	 * Copyright (c) 2013-present, Facebook, Inc.
	 *
	 * This source code is licensed under the MIT license found in the
	 * LICENSE file in the root directory of this source tree.
	 */

*jQuery.js*

	/*!
	 * jQuery JavaScript Library v3.3.1
	 * https://jquery.com/
	 *
	 * Includes Sizzle.js
	 * https://sizzlejs.com/
	 *
	 * Copyright JS Foundation and other contributors
	 * Released under the MIT license
	 * https://jquery.org/license
	 *
	 * Date: 2018-01-20T17:24Z
	 */

开源项目的**开发版本**与**生产版本**中都会保留文件注释，且必须对引用的其他开源代码进行说明。

## 文档注释（多行注释）

使用 ```/** ... */``` 作为文档API注释。包含描述与指定所有参数和返回值的类型和值的**注释标签**。

	/**
	 * Maps children that are typically specified as `props.children`.
	 *
	 * See https://reactjs.org/docs/react-api.html#react.children.map
	 *
	 * The provided mapFunction(child, key, index) will be called for each
	 * leaf child.
	 *
	 * @param {?*} children Children tree container.
	 * @param {function(*, int)} func The map function.
	 * @param {*} context Context for mapFunction.
	 * @return {object} Object containing the ordered map of results.
	 */
	function mapChildren(children, func, context) {

	  if (children == null) {
	    return children;
	  }

	  var result = [];
	  mapIntoWithKeyPrefixInternal(children, result, null, func, context);

	  return result;
	}

### 常用注释标签

```@param``` 指定参数的名称。您还可以包含参数的数据类型，使用大括号括起来，和参数的描述。

	/**
	 * @param content
	 */

**注释变量名 和 变量类型**

	/**
	 * @param {String} type
	 */

**注释变量名、变量类型 和 变量说明**

	/**
	 * @param {String} attrs Pipe-separated list of attributes
	 */

**连字符可以使注释阅读友好**

参数变量名与参数说明之间使用连字符 ```-```

	/**
	 * @param {object} partialState - Next partial state to be merged with state.
	 */

**对象参数属性描述**

*描述一个对象参数的属性*

	/**
	 * @param {Object} employee - The employee who is responsible for the project.
	 * @param {string} employee.name - The name of the employee.
	 * @param {string} employee.department - The employee's department.
	 */

同样，可以联想到如果假如 ```employee``` 参数是一个数组，这个数组中包含 ```name``` 和 ```department``` 元素，那么可以这么描述。

*描述参数的属性值在数组中*

	/**
	 * Assign the project to a list of employees.
	 * @param {Object[]} employees - The employees who are responsible for the project.
	 * @param {string} employees[].name - The name of an employee.
	 * @param {string} employees[].department - The employee's department.
	 */

**可选参数和默认值**

*JSDoc可选参数*

	/**
	 * @param {string} key - Key to be escaped.
	 */

*JSDoc可选参数和默认值*

	/**
	 * @param {Number} [index=0] - Somebody's name.
	 */

*Google Closure Compiler 可选参数*

	/**
	 * @param {string=} somebody - Somebody's name.
	 */

**多种类型参数**

下面的例子演示了如何使用类型的表达式来表示一个参数可以接受多种类型（或任何类型），还有一个参数可以被多次使用。

	/**
	 * @param {(string|string[])} [somebody=John Doe] - Somebody's name, or an array of names.
	 */

**允许任何类型**

	/**
	 * @param {*} component - A component that could contain a manual key.
	 */

**可重复使用的参数**

所有可变参数都是数字

	/**
	 * @param {...number} num - A positive or negative number.
	 */

管道字符 ```|``` 用来连接联合类型，联合类型用来表明参数可以有多个类型

	/** 
	 * @param {string|null|undefined} str 
	 */

因为参数是否非空很常见，因此有一个快捷方式用来标明联合类型是否包含 ```null```

	/** 
	 * @param {?string} str1 is a string or null 
	 * @param {?string|undefined} str2 is a string, null, or undefined. The ? 
	 *     prefix does not include undefined, so it must be included explicitly. 
	 */

除了基本类型的所有类型，如Object,Array和HTMLDocument默认都可以为空，这些类型统称为对象类型，因此 ```?``` 前缀对对象类型是多余的

	/** 
	 * @param {Document} doc1 is a Document or null because object types are nullable by default. 
	 * @param {?Document} doc2 is also a Document or null. 
	 */

如果要声明一个非空对象对开，可以用 ```!``` 前缀

	/** 
	 * @param {!Array} array must be a non-null Array 
	 * @param {!Array|undefined} maybeArray is an Array or undefined, but 
	 *     cannot be null. 
	 */

尽管一个方法中可以有任意多个可选参数，但是可选参数不应该出现在必须参数之前，如果出现在之前，代码必须写成如下形式

	/** 
	 * @param {string=} title Defaults to 'New Spreadsheet'. 
	 * @param {string} author 
	 */

大量的可靠参数，最好将其它移到一个必须的对象参数中

	/**  
	 * @param {{author: (string|undefined), title: (string|undefined), numRows: (number|undefined)  
	 */

**不定数量参数**

Closure不定数量参数

	/** 
	 * @param {string} category 
	 * @param {...} purchases 
	 * @return {number} 
	 */

注释函数类型值的可选和可变参数

	/** 
	 * @param {function(string, string=)} ……
	 * @param {function(string, ...[number]): number} ……
	 */

**回调函数**

使用 ```@callback``` 标签来定义回调类型，回调类型包含到 ```@param``` 中。

	/**
	 * This callback type is called `requestCallback` and is displayed as a global symbol.
	 *
	 * @callback requestCallback
	 * @param {number} responseCode
	 * @param {string} responseMessage
	 */
	
	/**
	 * Does something asynchronously and executes the callback on completion.
	 * @param {requestCallback} cb - The callback that handles the response.
	 */

```@return / @returns``` 返回值的类型和描述

	/**
	 * @return {object} Object containing the ordered map of results.
	 * @return {!number} The number of children in this subtree.
	 */

或者

	/**
	 * @returns {boolean} True if mounted, false otherwise.
	 * @returns {Element|Object|Boolean} The input node if acceptable, otherwise a falsy value
	 */

**更多示例**

	/**
	 * @param {?Function(*, Boolean)} callback - To invoke upon traversing each child.
	 * @param {String|Number} ref
	 * @param {?*} children - Children tree container.
	 * @param {Element|Object=} context
	 * @param {!ArrayLike<Element>} nodes
	 * @returns {Element|Object|Boolean} The input node if acceptable, otherwise a falsy value
	 */

	/**
	 * @param {?*} children - Children tree container.
	 * @param {!string} nameSoFar - Name of the key path so far.
	 * @param {!function} callback - Callback to invoke with each child found.
	 * @return {!number} The number of children in this subtree.
	 */

## 更多请参考

以下网站为本文参考，欢迎留言纠正。

[YUIDoc Syntax Reference](http://yui.github.io/yuidoc/syntax/index.html)  
[@use JSDoc](http://usejsdoc.org/)  
[Closure javascript注解](https://blog.csdn.net/zhangzq86/article/details/53406743)  
[Airbnb JavaScript Style Guide() { - 注释](https://github.com/yuche/javascript#comments)  
[原文Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript)  
[js/javascript代码注释规范与示例](https://blog.csdn.net/chenchunlin526/article/details/52821697)  
[Js注释](https://www.cnblogs.com/SLchuck/p/4376200.html)