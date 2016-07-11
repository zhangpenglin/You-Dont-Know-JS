# 你不知道的JavaScript: 类型和语法
# 第一章: 类型

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

然而，这种安全保护机制用在浏览器中处理多个脚本文件加载变量到共享的全局命名空间的时候，是一个有用的功能。（原句：Nevertheless, this safety guard is a useful feature when dealing with JavaScript in the browser, where multiple script files can load variables into the shared global namespace.）

**注意：**许多开发者认为不应该在全局命名空间中声明变量，任何的这一切都应该被包含在模块或者私有空间中。在理论上来说这个想法非常伟大，但是实际上几乎不可能；但它仍然是一个很好的值得努力奋斗的目标！幸运的是，ES6非常好的支持了模块，它最终会变得更加的实用。

举一个简单的例子，假设在你的程序中有“调试模式”，它被一个叫`DEBUG`的全局变量（也称为旗帜变量）控制着。在执行一个调试任务（如记录信息到控制台）之前，你可能要检查这个变量是否存在。全局变量声明`var DEBUG = true`只会存在于“debug.js”文件中，这个文件你只会在开发或测试的时候载入浏览器，在生产环境中就不会了。

然而，在你的代码中检查全局变量`DEBUG`的时候要特别小心，防止它抛出`ReferenceError`。在这种情况下，`typeof`的安全防护机制就是我们的好朋友了：）

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

即使你不处理用户定义的变量（如`DEBUG`），这种检查也是十分有用的。如果你正在做一个内置的API功能检查，你会发现用它来检查有助于防止抛出错误：

```js
if (typeof atob === "undefined") {
	atob = function() { /*..*/ };
}
```

**注意：**如果你正在为一个功能定义`polyfill`，如果它不存在，你可能想避免使用`var`声明`atob`。如果你在`if`语句中声明`var atob`，这个声明会被**提升**（参见本系列的**作用域和闭包**）到顶层作用域，即使`if`没有被执行（因为此时全局变量`atob`已经存在！）。在某些浏览器和一些特殊类型的全局内置变量（通常称为“宿主对象”，host objects），这种重复声明变量可能会抛出错误。省略`var`防止了这种变量**提升**。

另一种不使用`typeof`安全保护功能来检查全局变量的方法是检测全局对象的属性（全局变量会作为属性挂载在一个全局对象），在浏览器中一般是`window`对象。因此，上述的检查也可以这么做（相当地安全）：

```js
if (window.DEBUG) {
	// ..
}

if (!window.atob) {
	// ..
}
```

不像引用未声明的变量，如果你尝试访问对象的一个不存在的属性（即使是全局对象`window`），它不会抛出`ReferenceError`。

另一方面，一些开发者希望避免直接引用全局变量`window`，特别是当你的代码需要在多个JS环境中运行（不仅是浏览器，例如服务端的NodeJS），其中的全局变量的名称并不总叫`window`。

从技术上讲，即使你不使用全局变量（虽然这些情况不太常见），`typeof`的安全保护机制也很有用，有些开发者可能发现这种设计方法不太理想。假设有一段代码（别人可以复制粘贴到自己的程序中）或模块，在这段代码中你想要检查某个特定变量是否被定义过（这样你就可以使用它）：

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

`doSomethingCool()`函数中检查变量`FeatureXYZ`是否存在，如果存在，就用存在的那个函数；如果不存在，就用自己定义的。现在，如果有人将这个工具函数包含进自己的模块/程序中，就可以很安全的检测`FeatureXYZ`是否被定义过：

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

在这里，`FeatureXYZ`并不是全局变量，但是我们仍然可以使用`typeof`的安全保护机制来检测它是否安全可用。更重要的是，这里**没有**可用的对象（就像我们之间使用的全局变量`window.___`）来做检测，因此`typeof`是非常有帮助的。

其他的开发者可能更喜欢使用一种叫做“依赖注入”（dependency injection）的设计模式，不像`doSomethingCool()`需要隐式的检查`FeatureXYZ`是否在外部或周围定义过，它只需要明确的将这个依赖（作为参数）传递进来，如：

```js
function doSomethingCool(FeatureXYZ) {
	var helper = FeatureXYZ ||
		function() { /*.. default feature ..*/ };

	var val = helper();
	// ..
}
```

这里有很多的选项来设计这种功能。没有哪种模式是正确的或错误了——每种方法都需要权衡（there are various tradeoffs to each approach）。但总体来说，`typeof`的安全保护机制为我们提供了更多的选择。

## 小结

JavaScript有七个内置类型：`null`, `undefined`,  `boolean`, `number`, `string`, `object`, `symbol`。它们可以通过`typeof`操作符被标识。

变量没有类型，但是变量里面的值有类型。这些类型定义了值的固有行为。

许多开发者认为“undefined”和“undeclared”是大致相同的事情，但是在JavaScript中，它们是完全不同的事情。`undefined`是指声明了一个可以存放值的变量，只是当前没有值。“Undeclared”则是指一个没有被声明的变量。

很不幸的是JavaScript合并了这两个概念，不仅在错误信息（"ReferenceError: a is not defined"）上，而且在`typeof`的返回值上（都返回`"undefined"`）。

然而，`typeof`操作符的安全保护（防止错误被抛出）机制，在某些特定的情况下检测没有声明过的变量时很有帮助。
