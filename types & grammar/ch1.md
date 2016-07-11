# 你不知道的JavaScript: 类型和语法
# Chapter 1: 类型

大多数开发者会说，动态语言（如JS）没有**类型**。让我们来看看ES5.1规范 (http://www.ecma-international.org/ecma-262/5.1/) 中关于这个话题的解释：

> Algorithms within this specification manipulate values each of which has an associated type. The possible value types are exactly those defined in this clause. Types are further sub classified into ECMAScript language types and specification types.（本说明书中的算法操作中的每个值都有一个与之关联的类型。本节定义了所有的值类型。类型进一步细分为ECMAScript语言类型和规范类型。）
>
> An ECMAScript language type corresponds to values that are directly manipulated by an ECMAScript programmer using the ECMAScript language. The ECMAScript language types are Undefined, Null, Boolean, String, Number, and Object.（ECMAScript语言的类型直接对应到使用ECMAScript语言的程序员操作的值。ECMAScript语言的类型有Undefined，Null，Boolean，String，Number，and Object。）

如果你是强类型（静态类型）语言的拥泵，你可能会反对单词“类型”的这种用法。在这些语言中，“类型”的含义比在JS中意味着更多（原句：In those languages, "type" means a whole lot *more* than it does here in JS）。

有些人认为JS不应该声称有“类型”，它们应该被称作“标签”或“子类型”。

呸！我们将使用这种粗糙的定义（似乎和规范的提法是同一个）：**类型**是固有的，内置的一套特征，唯一标识一个特定值的行为并告诉**JS引擎**和**开发者**将它和其他值区分开。（原句：Bah! We're going to use this rough definition (the same one that seems to drive the wording of the spec): a *type* is an intrinsic, built-in set of characteristics that uniquely identifies the behavior of a particular value and distinguishes it from other values, both to the **engine** and to the **developer**.）

换句话说，如果引擎和开发者对待`42`（数字）不同于他们对待`"42"`（字符串），则这两个值的**类型**就不同——分别是`number`类型和`string`类型。当你使用`42`，你可能**打算**做一些数值操作，如数学计算。但是当你使用`"42"`，你**打算**做一些字符串操作，如输出到页面等。**这两个值就有不同的类型。**

这决不是一个完美的定义。但它的这种讨论已经足够了，并且它和JS如何描述自己保持一致。

## A Type By Any Other Name...

除了学术定义的分歧，为什么JavaScript有没有**类型**很重要？

正确理解每个**类型**和其内在的行为对于理解如何正确和准确地转换值到不同类型（参见第四章，强制转换）是必不可少的。几乎每个写过的JS程序在某个程度或形式上都要处理值的类型转换，因此带着责任心和信心去做这些事情是非常重要的。

如果你有数字`42`，但是你想把它当作字符串来用，如将`"2"`从位置`1`拉出，很显然你必须先将数字（强制）转换成字符串。

这似乎很简单。

但有很多不同的方法，这种强制转换可能发生。有些方法是明确的，很容易推导，并且很可靠。但是如果你不小心的话，强制转换可能以一种很奇怪和令人惊讶的方式发生。

强制转换的困惑也许是JavaScript开发人员最深刻的挫折之一。它经常因为**危险**而被批评，甚至被认为是语言的设计缺陷，应该避而远之。

在你完全理解JavaScript类型之后，我们会说明，为什么强制转换的“坏名声”在很大程度上是言过其实，我们会翻转你的视角，看到强制转换的能力和用处。但是在此之前，我们首先要牢牢掌握值和类型的相关知识。

## 内置类型

JavaScript定义了7种内置类型：

* `null`
* `undefined`
* `boolean`
* `number`
* `string`
* `object`
* `symbol` -- added in ES6!

**注意：**所有这些类型除了`object`都被称为“原始类型”。

`typeof`操作符检测给定值的类型，并始终返回7个字符串之一——很奇怪吧，它并没有跟我们刚刚列出的7个内置类型一一对应。（译者注：作者的意思是举的例子只返回了六种类型，没有和上面列举的七种内置类型一一对应。剧透一下，这是为后面讲解`null`和`function`埋下伏笔。原句：The `typeof` operator inspects the type of the given value, and always returns one of seven string values -- surprisingly, there's not an exact 1-to-1 match with the seven built-in types we just listed.）

```js
typeof undefined     === "undefined"; // true
typeof true          === "boolean";   // true
typeof 42            === "number";    // true
typeof "42"          === "string";    // true
typeof { life: 42 }  === "object";    // true

// added in ES6!
typeof Symbol()      === "symbol";    // true
```

如上所示，列出的6个类型都有相应的类型值，并返回一个与类型名同名的字符串。`Symbol`是ES6新增的数据类型，我们会在第三章讲解。

你可能已经注意到了，我把`null`从上面清单中排除了。它很**特殊**——特别是当它和`typeof`运算符结合的时候，

```js
typeof null === "object"; // true
```

如果它返回`"null"`就好了（给人感觉也是正确的！），但是JS这个原始的bug已经持续了将近20年，并且可能永远不会被修复，因为有太多的现存Web内容依赖这个bug的行为，如果修复了这个bug就会创建更多的bug，从而破坏很多的网络程序。

如果你想要用它的类型来检测`null`值，你需要一个复合条件判断：

```js
var a = null;

(!a && typeof a === "object"); // true
```

`null`是唯一一个既是“falsy”（假值，参见第四章）又在`typeof`检测下返回`"object"`的基本类型。

那么`typeof`返回的第七个字符串是什么？

```js
typeof function a(){ /* .. */ } === "function"; // true
```

我们很容易就想到`function`是JS中顶级的内置类型，特别是考虑到`typeof`操作符的这种行为。然而，如果你阅读了规范，你就会发现它其实是对象的一个“子类型”。具体来说，函数被称作是一个“可调用的对象”（callable object）——一个拥有内置属性`[[Call]]`的对象，允许它被调用。

知道函数实际上是对象这个事实是非常有用的。最重要的是，你知道它们可以具有属性。例如：

```js
function a(b,c) {
	/* .. */
}
```

这个函数对象有一个`length`属性，它的值是函数声明时正式命名的参数的个数。

```js
a.length; // 2
```

既然你声明的这个函数有两个正式命名的参数（`b`和`c`），**函数的长度**（**length of the function**）就是`2`。

那关于数组呢？它也是JS的原生类型，所以它也是一种特殊类型吗？

```js
typeof [1,2,3] === "object"; // true
```

不，它只是对象。数组拥有被数字索引的附加特性（相对于普通对象被字符串键索引）以及维护一个自动更新的`.length`属性，在这种情况下，把它当作对象的**子类型**是最合适的。

## 值类型

在JavaScript中，变量没有类型——**值才有类型**。变量可以在任何时间保存任何类型的值。

从另外一个角度思考JS类型就是JS没有“类型强制访问控制”（type enforcement），表现在引擎并不要求一个**变量**始终保持它的初始化值的类型。变量可以在一个赋值语句中保存`string`类型的值，在下一个赋值语句中又保存`number`类型的值。

值`42`的类型是`number`，它的**类型**不能改变。另外一个值，比如`"42"`是`string`类型，它可以是`number`类型的值`42`通过**强制转换**产生的。（参见第四章）

如果你对一个变量使用`typeof`，它并不是在问“这个变量是什么类型？”，尽管它看起来像，因为JS变量并没有类型。相反，它是在问：“变量**里面的值**是什么类型？”

```js
var a = 42;
typeof a; // "number"

a = true;
typeof a; // "boolean"
```

`typeof`运算符始终返回一个字符串，所以：

```js
typeof typeof 42; // "string"
```

第一个`typeof 42`返回`"number"`，而`typeof "number"`的结果是`"string"`。

### `undefined`与`undeclared`

当前没有值的变量，实际上它们的值是`undefined`。对这些变量使用`typeof`会返回`"undefined"`：

```js
var a;

typeof a; // "undefined"

var b = 42;
var c;

// later
b = c;

typeof b; // "undefined"
typeof c; // "undefined"
```

对大多数JS开发者来说，很容易认为单词`undefined`就是`undeclared`的代名词。然而，在JS中这两个概念是完全不同的。

一个`undefined`的变量是指在可访问作用域中声明了，但是在**当前**时间，变量里面没有存放值。相反，一个`undeclared`的变量是指在可访问作用域中没有声明的变量。

考虑如下：

```js
var a;

a; // undefined
b; // ReferenceError: b is not defined
```

最恼人的困惑是浏览器在这种情况下的给出的报错信息。如你所见，错误信息是“b is not defined”，这很容易也很合理的与“b is undefined”相混淆。再次强调，“undefined”和“is not defined”是很不同的事情。如果浏览器告诉我们的错误信息是“b is not found”或“b is not declared”，会减少很多的困惑！

`typeof`操作符有一个与未声明变量（undeclared variables）相关的特殊行为，这个特殊行为进一步加深了混乱。考虑如下：

```js
var a;

typeof a; // "undefined"

typeof b; // "undefined"
```

`typeof`运算符返回`"undefined"`，尽管这个变量是“undeclared”（或“not defined”，未定义的变量）。请注意，当我们执行`typeof b`的时候没有抛出任何错误，尽管`b`是一个未声明的变量。这是`typeof`的特殊安全保护机制造成的。

如果`typeof`针对未声明的变量返回“undeclared”，而不是将两种完全不同情况混为一谈都返回“undefined”，这该多好啊！

### typeof undeclared

Nevertheless, this safety guard is a useful feature when dealing with JavaScript in the browser, where multiple script files can load variables into the shared global namespace.

**Note:** Many developers believe there should never be any variables in the global namespace, and that everything should be contained in modules and private/separate namespaces. This is great in theory but nearly impossible in practicality; still it's a good goal to strive toward! Fortunately, ES6 added first-class support for modules, which will eventually make that much more practical.

As a simple example, imagine having a "debug mode" in your program that is controlled by a global variable (flag) called `DEBUG`. You'd want to check if that variable was declared before performing a debug task like logging a message to the console. A top-level global `var DEBUG = true` declaration would only be included in a "debug.js" file, which you only load into the browser when you're in development/testing, but not in production.

However, you have to take care in how you check for the global `DEBUG` variable in the rest of your application code, so that you don't throw a `ReferenceError`. The safety guard on `typeof` is our friend in this case.

```js
// oops, this would throw an error!
if (DEBUG) {
	console.log( "Debugging is starting" );
}

// this is a safe existence check
if (typeof DEBUG !== "undefined") {
	console.log( "Debugging is starting" );
}
```

This sort of check is useful even if you're not dealing with user-defined variables (like `DEBUG`). If you are doing a feature check for a built-in API, you may also find it helpful to check without throwing an error:

```js
if (typeof atob === "undefined") {
	atob = function() { /*..*/ };
}
```

**Note:** If you're defining a "polyfill" for a feature if it doesn't already exist, you probably want to avoid using `var` to make the `atob` declaration. If you declare `var atob` inside the `if` statement, this declaration is hoisted (see the *Scope & Closures* title of this series) to the top of the scope, even if the `if` condition doesn't pass (because the global `atob` already exists!). In some browsers and for some special types of global built-in variables (often called "host objects"), this duplicate declaration may throw an error. Omitting the `var` prevents this hoisted declaration.

Another way of doing these checks against global variables but without the safety guard feature of `typeof` is to observe that all global variables are also properties of the global object, which in the browser is basically the `window` object. So, the above checks could have been done (quite safely) as:

```js
if (window.DEBUG) {
	// ..
}

if (!window.atob) {
	// ..
}
```

Unlike referencing undeclared variables, there is no `ReferenceError` thrown if you try to access an object property (even on the global `window` object) that doesn't exist.

On the other hand, manually referencing the global variable with a `window` reference is something some developers prefer to avoid, especially if your code needs to run in multiple JS environments (not just browsers, but server-side node.js, for instance), where the global variable may not always be called `window`.

Technically, this safety guard on `typeof` is useful even if you're not using global variables, though these circumstances are less common, and some developers may find this design approach less desirable. Imagine a utility function that you want others to copy-and-paste into their programs or modules, in which you want to check to see if the including program has defined a certain variable (so that you can use it) or not:

```js
function doSomethingCool() {
	var helper =
		(typeof FeatureXYZ !== "undefined") ?
		FeatureXYZ :
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

`doSomethingCool()` tests for a variable called `FeatureXYZ`, and if found, uses it, but if not, uses its own. Now, if someone includes this utility in their module/program, it safely checks if they've defined `FeatureXYZ` or not:

```js
// an IIFE (see "Immediately Invoked Function Expressions"
// discussion in the *Scope & Closures* title of this series)
(function(){
	function FeatureXYZ() { /*.. my XYZ feature ..*/ }

	// include `doSomethingCool(..)`
	function doSomethingCool() {
		var helper =
			(typeof FeatureXYZ !== "undefined") ?
			FeatureXYZ :
			function() { /*.. default feature ..*/ };

		var val = helper();
		// ..
	}

	doSomethingCool();
})();
```

Here, `FeatureXYZ` is not at all a global variable, but we're still using the safety guard of `typeof` to make it safe to check for. And importantly, here there is *no* object we can use (like we did for global variables with `window.___`) to make the check, so `typeof` is quite helpful.

Other developers would prefer a design pattern called "dependency injection," where instead of `doSomethingCool()` inspecting implicitly for `FeatureXYZ` to be defined outside/around it, it would need to have the dependency explicitly passed in, like:

```js
function doSomethingCool(FeatureXYZ) {
	var helper = FeatureXYZ ||
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

There are lots of options when designing such functionality. No one pattern here is "correct" or "wrong" -- there are various tradeoffs to each approach. But overall, it's nice that the `typeof` undeclared safety guard gives us more options.

## 小结

JavaScript has seven built-in *types*: `null`, `undefined`,  `boolean`, `number`, `string`, `object`, `symbol`. They can be identified by the `typeof` operator.

Variables don't have types, but the values in them do. These types define intrinsic behavior of the values.

Many developers will assume "undefined" and "undeclared" are roughly the same thing, but in JavaScript, they're quite different. `undefined` is a value that a declared variable can hold. "Undeclared" means a variable has never been declared.

JavaScript unfortunately kind of conflates these two terms, not only in its error messages ("ReferenceError: a is not defined") but also in the return values of `typeof`, which is `"undefined"` for both cases.

However, the safety guard (preventing an error) on `typeof` when used against an undeclared variable can be helpful in certain cases.
