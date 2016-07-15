# 你不知道的JavaScript: 类型和语法
# 第二章：值

`array`、`string`和`number`是任何程序的最基本的构建模块，但JavaScript的这些类型有一些独特的特点，可能会让你感到愉悦，也可能让你感到混淆。

让我们来看看几个JS中的内置类型，并探讨如何更充分地理解和正确使用它们。

## 数组

相比于其他强类型的语言，JavaScript的数组可以容纳任何类型的值，从`string`到`number`再到`object`甚至是另外一个`array`（通过这种方式你可以得到多维数组）。

```js
var a = [ 1, "2", [3] ];

a.length;		// 3
a[0] === 1;		// true
a[2][0] === 3;	// true
```

你并不需要调整数组的大小（参见第三章的数组），你只需要声明它们，然后将你认为合适的值添加进去就行：

```js
var a = [ ];

a.length;	// 0

a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];

a.length;	// 3
```

**警告：**对`array`上的值使用`delete`操作，会将那个槽（slot）上的值从数组中删除（变为`undefined`），但即使你删除了所有元素，它并不会改变数组的`length`属性，所以请小心这个坑！我们会在第五章详细讲解`delete`操作符。

创建“疏松”数组（留下或创建空的槽）的时候要特别小心：

```js
var a = [ ];

a[0] = 1;
// no `a[1]` slot set here
a[2] = [ 3 ];

a[1];		// undefined

a.length;	// 3
```

虽然这可以工作，但是留下来的“空槽”可能会导致一些容易混淆的行为。虽然槽中的值看起来是`undefined`，当它的行为和明确设置`a[1] = undefined`是不一样的。请参阅第三章“数组”获取更多信息。

数组是数字索引的（如你所期望的），但坑爹的是数组也是对象，所以你可以添加字符串的键或属性到数组中（但是这不会改变数组的长度）：

```js
var a = [ ];

a[0] = 1;
a["foobar"] = 2;

a.length;		// 1
a["foobar"];	// 2
a.foobar;		// 2
```

然而，这里有个神坑你要特别注意，如果你打算将一个能通过强制转换成十进制数字的字符串作为数组的键传入，它会假定你想用数字下标而不是字符串键！

```js
var a = [ ];

a["13"] = 42;

a.length; // 14
```

一般来说，把`string`类型的键或属性加入到`array`中并不是一个好主意。使用`object`来保存键或属性的值，使用`array`来严格用数字索引值。

### 类数组对象

在有些场合，你需要把一个类数组对象（数字索引的一组值的集合）转换成一个真的数组，这样你就可以调用数组的工具函数（比如`indexOf(..)`，`concat(..)`，`forEach(..)`等）来操作这组值。

例如，各种DOM查询操作返回的DOM元素列表，它们并不是真的数组而是类数组对象（遵循转换协议）。另外一个常见的例子是函数暴露出的`arguments`（类数组）对象（在ES6中被废弃，不推荐使用），它把访问参数放在一个列表中。

非常参见的转换方法是使用数组的工具函数`slice(..)`来完成这项操作：

```js
function foo() {
	var arr = Array.prototype.slice.call( arguments );
	arr.push( "bam" );
	console.log( arr );
}

foo( "bar", "baz" ); // ["bar","baz","bam"]
```

如果不带参数调用`slice()`（正如上面代码片段所示），它所产生的效果就是复制数组对象（或者类数组对象）并返回。

在ES6中有一个内置的工具`Array.from(..)`能够完成同样的工作：

```js
...
var arr = Array.from( arguments );
...
```

**注意：**`Array.from(..)`有几个强大的功能，在本系列标题为“超越ES6”中会讲解更多的细节。

## 字符串

大家普遍认为字符串其实就是字符数组。但是其底层实现可能使用或不使用数组，我们需要认识到JavaScript中的字符串与字符数组是不一样的。它们仅仅是表面上相似。

例如，让我们考虑下面两个值：

```js
var a = "foo";
var b = ["f","o","o"];
```

字符串确实跟数组很相似（但那仅仅是表面上相似，你可以把它理解为我们上面提到过的类数组对象），例如，它们都有`length`属性，`indexOf(..)`方法（数组是在ES5才有的）和`concat(..)`方法：

```js
a.length;							// 3
b.length;							// 3

a.indexOf( "o" );					// 1
b.indexOf( "o" );					// 1

var c = a.concat( "bar" );			// "foobar"
var d = b.concat( ["b","a","r"] );	// ["f","o","o","b","a","r"]

a === c;							// false
b === d;							// false

a;									// "foo"
b;									// ["f","o","o"]
```

因此它俩基本上就是“字符数组”，对吗？**不完全是**：

```js
a[1] = "O";
b[1] = "O";

a; // "foo"
b; // ["f","O","o"]
```

JavaScript中的字符串是不可修改的，而数组是可以修改的。此外，`a[1]`这种下标访问字符的形式在JavaScript中没有广泛使用。老版本的IE不支持这种语法格式（但新版本支持）。正确的做法是使用`a.charAt(1)`这种形式。

字符串不允许修改的另外一个后果就是，没有任何一个改变其内容的字符串方法能够直接修改字符串，而是必须创建并返回新的字符串。相比之下，很多修改数组内容的方法确实是直接修改了数组的内容。

```js
c = a.toUpperCase();
a === c;	// false
a;			// "foo"
c;			// "FOO"

b.push( "!" );
b;			// ["f","O","o","!"]
```

数组的许多方法在处理字符串上非常方便，然而字符串却没有这些方法，但是我们可以“借用”数组的非修改方法来操作字符串：（原句：Also, many of the `array` methods that could be helpful when dealing with `string`s are not actually available for them, but we can "borrow" non-mutation `array` methods against our `string`:）

```js
a.join;			// undefined
a.map;			// undefined

var c = Array.prototype.join.call( a, "-" );
var d = Array.prototype.map.call( a, function(v){
	return v.toUpperCase() + ".";
} ).join( "" );

c;				// "f-o-o"
d;				// "F.O.O."
```

我们来看另外一个例子：反转一个字符串（顺便提一句，这是一个很常见的JavaScript面试题！）。数组有`reverse()`方法能立即修改数组内容，但是字符串并没有：

```js
a.reverse;		// undefined

b.reverse();	// ["!","o","O","f"]
b;				// ["!","o","O","f"]
```

不幸的是，这种“借用”的方式在数组修改方法上不起作用，因为字符串是不可修改的，因此不能直接修改其内容：

```js
Array.prototype.reverse.call( a );
// still returns a String object wrapper (see Chapter 3)
// for "foo" :(
```

另一个解决方法（又称hack）是先将字符串转换成数组，执行所需的操作，然后再把它转回字符串。

```js
var c = a
	// split `a` into an array of characters
	.split( "" )
	// reverse the array of characters
	.reverse()
	// join the array of characters back to a string
	.join( "" );

c; // "oof"
```

看起来很丑陋，确实如此。然而，对简单字符串来说，它确实能工作，因此如果你需要快速操作字符串的话，这种方法就能够完成任务。

**警告：**特别小心！这种方法不能用于操作含有复杂字符（Unicode）的字符串（特殊符号，多字节字符等）。你需要更复杂工具库（考虑到了Unicode字符）来准确地实现这样的操作。你可以参考Mathias Bynens的实现：**Esrever** (https://github.com/mathiasbynens/esrever) . 

另一种解决方法是：如果你经常把你的字符串当作字符数组来使用，那你最好直接把它存入数组而不是字符串中。这样你就没必要每次都将字符串转换为数组。当你真正需要字符串表示时，你可以调用数组的`join("")`方法。

## 数字

JavaScript只有一个数值类型：`number`。这个类型包括十进制“整数”和小数。我用引号引起“整数”，是因为在JS中没有真正的整数，而其他语言中有。未来可能会改变，但现在，我们只有`number`类型。

所以在JS中，一个“整数”就是没有小数的十进制数字。也就是说，`42.0`和“整数”`42`是等价的。

像大多数现代语言，包括几乎所有的脚本语言，JavaScript的`number`是基于“IEEE 754”标准实现的，通常称为“浮点数”。JavaScript具体使用标准的“双精度”格式（又名“64位二进制”）。

在网上有许多有关二进制浮点数是如何在内存中存储以及各种选项带来的影响的讨论。因为了解内存位模式对于如何在JS中正确使用`number`并非绝对必要，我们把它作为一个练习，有兴趣的读者可以进一步深入IEEE 754的细节。（原句：There are many great write-ups on the Web about the nitty-gritty details of how binary floating-point numbers are stored in memory, and the implications of those choices. Because understanding bit patterns in memory is not strictly necessary to understand how to correctly use `number`s in JS, we'll leave it as an exercise for the interested reader if you'd like to dig further into IEEE 754 details.）

### 数值语法

在JavaScript中数字字面量用十进制数字表示。例如：
```js
var a = 42;
var b = 42.3;
```

十进制值的左边部分，如果是`0`，可以省略：

```js
var a = 0.42;
var b = .42;
```

同理，十进制值的尾部（小数点之后的数字），如果是`0`，也可以省略：

```js
var a = 42.0;
var b = 42.;
```

**警告：**`42.`这种形式很罕见，如果你不想给看你代码的人造成混淆，请别这么写。尽管这么写是正确且有效的。

默认情况下，大多数的`number`作为十进制输出的时候，会去掉小数点后边的`0`。如下：

```js
var a = 42.300;
var b = 42.0;

a; // 42.3
b; // 42
```

非常大或者非常小的`number`默认以指数的形式输出，与`toExponential()`方法的输出格式相同，比如：

```js
var a = 5E10;
a;					// 50000000000
a.toExponential();	// "5e+10"

var b = a * a;
b;					// 2.5e+21

var c = 1 / a;
c;					// 2e-11
```

因为`number`值会被装箱成`Number`对象（参见第三章），`number`值可以访问`Number.prototype`（参见第三章）的内置方法。例如，`toFixed(..)`方法允许你指定多少位十进制小数来展示输出：

```js
var a = 42.59;

a.toFixed( 0 ); // "43"
a.toFixed( 1 ); // "42.6"
a.toFixed( 2 ); // "42.59"
a.toFixed( 3 ); // "42.590"
a.toFixed( 4 ); // "42.5900"
```

注意，输出实际上是数字的字符串表现形式，如果你要求更多的小数位数，它会在右边用`0`填充。（原句：Notice that the output is actually a `string` representation of the `number`, and that the value is `0`-padded on the right-hand side if you ask for more decimals than the value holds.）

`toPrecision(..)`是类似的，它是指定多少**有效位数**来表示值的：

```js
var a = 42.59;

a.toPrecision( 1 ); // "4e+1"
a.toPrecision( 2 ); // "43"
a.toPrecision( 3 ); // "42.6"
a.toPrecision( 4 ); // "42.59"
a.toPrecision( 5 ); // "42.590"
a.toPrecision( 6 ); // "42.5900"
```

你可以不使用带值的变量来访问这些方法；可以直接用数字字面量来访问。但是你得特别小心`.`操作符。因为`.`是一个有效的数字字符，如果可能的话，它会优先被解释为数字直面量的一部分，而不是解释成属性访问符。

```js
// invalid syntax:
42.toFixed( 3 );	// SyntaxError

// these are all valid:
(42).toFixed( 3 );	// "42.000"
0.42.toFixed( 3 );	// "0.420"
42..toFixed( 3 );	// "42.000"
```

`42.toFixed(3)`是无效的语法，因为`.`被解释成`42.`字面量（这是有效的，上面提到过！）的一部分，因此这里没有属性操作符`.`来访问`.toFixed`。

`42..toFixed(3)`可以工作是因为第一个`.`是数字的一部分，第二个`.`是属性操作符。但它可能看起来很奇怪，实际上在JavaScript代码中很少见到类似的代码。事实上，直接通过字面量来访问方法不太常见。但不常见不意味着**不好的**或**错误的**。

**注意：**这里有许多库扩展了内置的`Number.prototype`（参见第三章），提供了对数字的额外操作，所以在这种情况下，使用类似`10..makeItRain()`掀起10秒下钱雨的动画这种格式很有效，或是其他像这么愚蠢的东西（原句：it's perfectly valid to use something like `10..makeItRain()` to set off a 10-second money raining animation, or something else silly like that.）。

技术上来说，下面这个格式也是有效的（注意42后面的空格）：

```js
42 .toFixed(3); // "42.000"
```

然而，这种特殊形式的数字字面量，**是特别混乱的编码风格**，它一无是处，除了混淆其他开发者（和未来的你）。请避免使用它！

数字也可以使用指数形式，代表较大的数字，比如：

```js
var onethousand = 1E3;						// means 1 * 10^3
var onemilliononehundredthousand = 1.1E6;	// means 1.1 * 10^6
```

数字字面量也可以表示成其他进制，如二进制、八进制和十六进制。

这些格式在当前的JavaScript版本中可以工作：

```js
0xf3; // hexadecimal for: 243
0Xf3; // ditto

0363; // octal for: 243
```

**注意：**在ES6的严格模式下（`strict` mode），`0363`这种八进制形式的字面量是不允许的（下面会给出最新的格式）。`0363`这种格式在非严格模式下依然是可用的，但是你应该停止使用它，为了对未来更友好（因此你现在就应该开始使用严格模式）。

对于ES6来说，下面的新格式也是有效的：

```js
0o363;		// octal for: 243
0O363;		// ditto

0b11110011;	// binary for: 243
0B11110011; // ditto
```

为了你的同事着想，千万不要使用`0O363`这种格式。把`0`和大写的`O`放在一起就是想搞混淆。始终使用小写字符`0x`、`0b`和`0o`。

### 十进制小数

使用二进制浮点数最著名的副作用（请记住，是所有使用IEEE 754的语言——不仅是JavaScript）：

```js
0.1 + 0.2 === 0.3; // false
```

在数学计算上，我们知道表达式的结果应该是`true`。但是这里为什么是`false`？

简单地说，`0.1`和`0.2`的二进制浮点表示并不是完全精确的，所以当它们相加，得到的结果并不是精确的`0.3`。而是非常接近`0.3`的`0.30000000000000004`，但是如果你比较失败，非常接近是没有用的（原句：but if your comparison fails, "close" is irrelevant）。

**注意：**有人会想，为什么不把JavaScript的数字实现切换到一种对所有值都精确表示的形式？这些年出现了很多的替代实现。至今没有一个被采用的，也许永远不会被采用。看起来似乎很容易，你只要挥一挥手，说：“修复这个bug了！”，但是它真的没有这么容易。如果真这么容易，肯定早就改变了。（原句：**Note:** Should JavaScript switch to a different `number` implementation that has exact representations for all values? Some think so. There have been many alternatives presented over the years. None of them have been accepted yet, and perhaps never will. As easy as it may seem to just wave a hand and say, "fix that bug already!", it's not nearly that easy. If it were, it most definitely would have been changed a long time ago.）

现在的问题是，如果有些数字不能精确表示，那是不是意味着我们就不能使用数字了？**当然不是！**

这里有一些应用你需要特别小心，尤其是进行十进制小数运算的时候。也有大量的（也许是大多数）应用只需要处理整数，而且，处理的最大值在百万或者亿万。在这些应用中可以（永远）很安全的使用JS数值运算。

如果我们确实需要比较两个数字，如`0.1 + 0.2`和`0.3`，即使我们已经知道简单的相等测试会失败。

最普遍接受的做法是使用一个微小的**舍入误差**值作为比较的公差值。这个微小的值通常被称为“机器精度”，在JavaScript中通常是`2^-52` (`2.220446049250313e-16`)这个值。

在ES6中，`Number.EPSILON`就是这个预定义的公差值，你可以直接使用它。如果你想要在ES6之前的版本中安全使用它，你也可以很容易的进行polyfill。

```js
if (!Number.EPSILON) {
	Number.EPSILON = Math.pow(2,-52);
}
```

我们可以使用这个`Number.EPSILON`来比较两个数字是否相等（在舍入误差范围内）：

```js
function numbersCloseEnoughToEqual(n1,n2) {
	return Math.abs( n1 - n2 ) < Number.EPSILON;
}

var a = 0.1 + 0.2;
var b = 0.3;

numbersCloseEnoughToEqual( a, b );					// true
numbersCloseEnoughToEqual( 0.0000001, 0.0000002 );	// false
```

浮点数的最大值大致是`1.798e+308`（它是真的，真的，真的很巨大的！重要的事情说三遍），预定义的变量`Number.MAX_VALUE`就是这个值。在最小值上，`Number.MIN_VALUE`大概是`5e-324`，它不是负数，但是它非常接近于零！

### 整数安全范围

我们现在知道数字是如何表示的，所以你应该明白数字中“整数”有一个**安全范围**值，很显然它比`Number.MAX_VALUE`要小。（原句：Because of how `number`s are represented, there is a range of "safe" values for the whole `number` "integers", and it's significantly less than `Number.MAX_VALUE`.）

可以**安全地**表示（就是遵循规定，能够准确无误的表示，原句：that is, there's a guarantee that the requested value is actually representable unambiguously）的最大整数是`2^53 - 1`，即`9007199254740991`。如果你插入逗号数一数，你会发现它刚刚超过9万亿。这对数字来说是一个相当大的范围。

这个值实际上是ES6自动预定义的，即`Number.MAX_SAFE_INTEGER`。毫不奇怪，这里也有一个最小值，`-9007199254740991`，它在ES6中定义为`Number.MIN_SAFE_INTEGER`。

JS程序面临处理大数字的场景主要是来自数据库64位的ID等，64位数字不能用数字类型准确表示，因此必须存储在JavaScript字符串中然后进行展示。

幸运的是，在如此大的ID数字上进行数值运算并不常见。但是，如果你**确实**需要对这么大的数字进行数学运算，就目前情况，你需要使用**大数字**工具处理包。大数字处理可能在未来的JavaScript版本中得到官方支持。

### 整数检测

想要测试一个值是不是整数，你可以使用ES6指定的`Number.isInteger(..)`方法：

```js
Number.isInteger( 42 );		// true
Number.isInteger( 42.000 );	// true
Number.isInteger( 42.3 );	// false
```

在ES6之前的版本，你可以这样polyfill方法`Number.isInteger(..)`：

```js
if (!Number.isInteger) {
	Number.isInteger = function(num) {
		return typeof num == "number" && num % 1 == 0;
	};
}
```

想要测试一个值是不是**安全整数**，使用ES6指定的`Number.isSafeInteger(..)`：

```js
Number.isSafeInteger( Number.MAX_SAFE_INTEGER );	// true
Number.isSafeInteger( Math.pow( 2, 53 ) );			// false
Number.isSafeInteger( Math.pow( 2, 53 ) - 1 );		// true
```

在ES6之前，你可以这么polyfill方法`Number.isSafeInteger(..)`：

```js
if (!Number.isSafeInteger) {
	Number.isSafeInteger = function(num) {
		return Number.isInteger( num ) &&
			Math.abs( num ) <= Number.MAX_SAFE_INTEGER;
	};
}
```

### 32位（有符号）整数

虽然整数的安全范围可以达到约900万亿，但还有一些数字操作（如位运算符），这些只适用于32位数字，以这种方式使用的数字的“安全范围”要小得多。

这个范围是`Math.pow(-2,31)`（`-2147483648`，大约-21亿）到`Math.pow(2,31)-1`（`2147483647`，大约21亿）。

要把一个数字强制转换成32位有符号整数值，使用`a | 0`。这可以工作，因为`|`位运算符仅适用于32位整数（这意味着它可以只关注32位，其他位将丢失）。所以，与0进行或运算本质上是一个无操作符位运算（请忽略我的翻译，看原句：Then, "or'ing" with zero is essentially a no-op bitwise speaking）。

**注意：**某些特殊值（我们将在下一节介绍），如`NaN`和`Infinity`不是“32位安全”的，因为当这些值传给位操作符时，它们会经过一个抽象操作`ToInt32`（参见第四章）变成简单的`+0`值，然后才进行位运算操作。

## 特殊值

这里有几个特殊的值，横跨各种类型，我们在这**提醒**JS开发者需要特别注意，并且正常使用。

### 非值的值（The Non-value Values）

对于`undefined`类型，有且只有一个值：`undefined`。对于`null`类型，有且只有一个值：`null`。因此对于这两者，这个标签既是类型又是它对应的值。

`undefined`和`null`经常被认为是空值或非值。有些开发者更喜欢用细微差别来区分它们。例如：

* `null`是一个空值
* `undefined`是一个缺失值

或：

* `undefined`表示当前变量还没有值
* `null`表示曾经有值，但现在没有

无论你选择如何“定义”和使用这两个值，`null`是一个特殊的关键字，而不是一个标识符，因此你不能把它当作变量然后赋值给它（你为什么想这么做！？）。然而，`undefined`**是**（很不幸地！）一个标识符。

### Undefined

在非严格模式中，你可以给全局标识符`undefined`赋值（这是非常不明智的做法）：

```js
function foo() {
	undefined = 2; // really bad idea!
}

foo();
```

```js
function foo() {
	"use strict";
	undefined = 2; // TypeError!
}

foo();
```

在非严格模式和严格模式中，你可以创建一个名为`undefined`的局部变量。但同样，这是一个非常糟糕的想法！

```js
function foo() {
	"use strict";
	var undefined = 2;
	console.log( undefined ); // 2
}

foo();
```

**好朋友永远不会让好朋友覆盖`undefined`。**（原句：**Friends don't let friends override `undefined`.** Ever.）

#### `void`操作符

`undefined`是一个内置的标识符存储了内置的`undefined`值，另外一种获取该值的方法是`void`操作符。

表达式`void ___`会“清空”任何值，因此表达式的结果总是`undefined`值。它不会修改现有的值；它只是确保`void`表达式不会返回任何值。

```js
var a = 42;

console.log( void a, a ); // undefined 42
```

按照惯例（主要来自C语言编程），单独使用`void`来表示`undefined`值，你需要使用`void 0`（尽管`void true`或其他`void`表达式都可以做同样的事情）。其实`void 0`、`void 1`和`undefined`实际上没有什么不同。

在一些其他情况下，`void`操作符也很有用，如果你需要确保一个表达式不返回任何值（即使这个函数会产生副作用）。

例如：

```js
function doSomething() {
	// note: `APP.ready` is provided by our application
	if (!APP.ready) {
		// try again later
		return void setTimeout( doSomething, 100 );
	}

	var result;

	// do some other stuff
	return result;
}

// were we able to do it right away?
if (doSomething()) {
	// handle next tasks right away
}
```

在这里，`setTimeout(..)`函数返回一个数字值（这是计时器的唯一标识符，你取消计时器的时候要用到），但是我们想将这个结果清空，这样我们函数的返回值就通不过`if`语句（返回值为假）。

很多开发者更喜欢将这两个动作分开，虽然做同样的事情，但是不使用`void`操作符：

```js
if (!APP.ready) {
	// try again later
	setTimeout( doSomething, 100 );
	return;
}
```

一般情况下，如果有个地方有一个值（从一些表达式返回的），然后你发现用`undefined`代替它会很有用，请用`void`操作符。这在你的程序中可能并不常见，但在极少数情况下你会需要它，它会变得非常有用。

### 特殊数字

数字类型包括几个特殊的值。我们将详细地看看每个特殊值。

#### 非数字的数字（The Not Number, Number）

任何的数学运算操作，如果操作符两边不都是数字（可以被解释为常规的数字，十进制或十六进制都行）将会导致操作未能产生一个有效的数字，在这种情况下，你会得到一个`NaN`值。

`NaN`字面上的意思是“not a `number`”，然而这个描述非常差劲并且容易误导人，我们下面就会看到。使用“invalid number”（无效的数字）、“failed number”（失败的数字）甚至“bad number”（不好的数字）来表述`NaN`会比“not a number”（不是一个数字）更加准确。

举个例子：

```js
var a = 2 / "foo";		// NaN

typeof a === "number";	// true
```

换句话说：“not-a-number的类型是数字！”。这名字和语义真会误导人！

`NaN`是一种“标记值”（一个正常的值，但是它被赋予了特殊含义），它表示`number`集合中一种特殊的错误情况。这种错误情况，在本质上是说：“我试图执行一个数学运算，但失败了，所以我给你这个失败的数字结果。”

因此如果你想测试你变量的值是不是这个特殊的失败数字`NaN`，你可能会认为你可以直接将它与`NaN`本身进行比较，就像你对其他值的做法一样，如`null`和`undefined`。Too young too simple！

```js
var a = 2 / "foo";

a == NaN;	// false
a === NaN;	// false
```

`NaN`是一个非常特殊的值，因为它永远不会等于另外一个`NaN`值（即，它永远不会等于它自己）。事实上，它是唯一不遵守自反性（不带身份特征`x === x`）的值。因此`NaN !== NaN`，有点怪，对吧？（原句：`NaN` is a very special value in that it's never equal to another `NaN` value (i.e., it's never equal to itself). It's the only value, in fact, that is not reflexive (without the Identity characteristic `x === x`). So, `NaN !== NaN`. A bit strange, huh?）

既然我们不能对`NaN`进行比较（因为比较总是失败），那我们如何检测它？

```js
var a = 2 / "foo";

isNaN( a ); // true
```

很简单吧！我们使用内置的全局工具函数`isNaN(..)`来告诉我们一个值是不是`NaN`。问题解决了！

事情并没有这么简单。

`isNaN(..)`工具函数有个致命的缺陷。它仅仅只是取`NaN`的字面意思（“Not a Number”，不是一个数字），因此这个函数的工作就是简单的检测传递进来值是不是一个数字。但是这并不准确。（译者注：其实`isNaN(..)`函数通常用于检测`parseFloat()`和`parseInt()`的结果，以判断它们表示的是否是合法的数字。所以这个函数认为传递进来的值只有`NaN`和正常数字两种情况，并没有考虑到传递进来的还可能是字符串或其他非数字类型的值）

```js
var a = 2 / "foo";
var b = "foo";

a; // NaN
b; // "foo"

window.isNaN( a ); // true
window.isNaN( b ); // true -- ouch!
```

显然，`"foo"`只是字面上**not a `number`**，但它根本不是`NaN`！这个bug在JS早期（超过19年）就一直存在。

在ES6中终于提供了一个替代工具：`Number.isNaN(..)`。你可以简单的polyfill它，这样你现在就可以安全的检测`NaN`了，即使在ES6版本之前的浏览器中：

```js
if (!Number.isNaN) {
	Number.isNaN = function(n) {
		return (
			typeof n === "number" &&
			window.isNaN( n )
		);
	};
}

var a = 2 / "foo";
var b = "foo";

Number.isNaN( a ); // true
Number.isNaN( b ); // false -- phew!
```

实际上，我们可以用一种更简单的方法来pollfill实现`Number.isNaN(..)`，利用`NaN`不等于它自己这个特有事实。`NaN`是JS语言中**唯一**不等于它自己的值，所有其他值总是**等于自身**。

因此：

```js
if (!Number.isNaN) {
	Number.isNaN = function(n) {
		return n !== n;
	};
}
```

很奇怪，对吧？但它确实能工作！

`NaN`在许多现实世界的JS程序中存在，无论是有意还是无意产生的。因此使用一个可靠的测试是必须的，就像ES6提供的`Number.isNaN(..)`（或polyfill的）能够准确识别`NaN`。

如果你的程序正在使用`isNaN(..)`，可悲的现实是你的程序存在bug，即使现在你还没被它坑过！

#### 无穷大

看如下这个例子，传统的编译型语言（如C语言）开发者可能已经习惯看到编译错误或运行时异常（如“Divide by zero”）：

```js
var a = 1 / 0;
```

然后在JS中，这种操作是明确定义的，会返回结果`Infinity`（又名`Number.POSITIVE_INFINITY`）。不出所料：

```js
var a = 1 / 0;	// Infinity
var b = -1 / 0;	// -Infinity
```

如你所见，进行除0运算，除法操作符两边有一边是负的（不能两边都是负的！）就会产生结果 `-Infinity`（又名`Number.NEGATIVE_INFINITY`）。

JS使用了有限的数字表示（IEEE 754浮点表示法，我们在前面介绍过），这违背了纯数学，在进行加法或减法运算的时候可能会溢出，在这种情况下你会得到`Infinity`或`-Infinity`。（原句：JS uses finite numeric representations (IEEE 754 floating-point, which we covered earlier), so contrary to pure mathematics, it seems it *is* possible to overflow even with an operation like addition or subtraction, in which case you'd get `Infinity` or `-Infinity`.）

举个例子：

```js
var a = Number.MAX_VALUE;	// 1.7976931348623157e+308
a + a;						// Infinity
a + Math.pow( 2, 970 );		// Infinity
a + Math.pow( 2, 969 );		// 1.7976931348623157e+308
```

根据规范，如果数学运算操作（如加法）导致结果值太大而不能被显示，IEEE 754的“舍入到最近”（round-to-nearest）模式指定了结果应该是什么。简单来说，`Number.MAX_VALUE + Math.pow( 2, 969 )`比`Infinity`更加靠近`Number.MAX_VALUE`，所以进行“向下舍去”，而`Number.MAX_VALUE + Math.pow( 2, 970 )`更靠近`Infinity`，所以进行“向上舍入”。

如果你对这个想太多，你会更加懵逼。所以，讲真，请适可而止！

一旦你的运算结果溢出，产生了“无穷大”（两个无穷中的任意一个），你就不能回头了。用诗意的话来阐述，就是，你可以从有限到无限，但是不能从无限回退到有限。

有个哲学性的问题：“无穷大除以无穷大的结果是什么”。我们天真的大脑可能会说“1”或“无穷大”。事实证明都是错的。无论是数学意义上还是在JavaScript中，`Infinity / Infinity`并不是一个已定义的操作。在JS中，你会得到`NaN`。

那正的有限值除以无穷大的结果是多少？这还是很简单的，当然是`0`。那负的有限值除以无穷大的结果是多少？请继续阅读！

#### 零

JavaScript有两个正常的零，`0`（也称为正零`+0`）和负零`-0`，有些数学头脑的读者可能会感到混淆。在我们解释为什么`-0`存在之前，我们应该研究JS如何处理它，因为它可能会相当混乱。

除了字面上指定为`-0`，特定的运算结果也会产生负零。例如：

```js
var a = 0 / -3; // -0
var b = 0 * -3; // -0
```

加减运算不会产生负零的结果。

负零在开发者控制台通常显示成`-0`，在以前可不是这样的，所以你如果你的浏览器很旧，它仍然会显示成`0`。

然而，根据规范，如果你尝试将一个零值转换成字符串，不管是正零还是负零，都会被转换成`"0"`。

```js
var a = 0 / -3;

// (some browser) consoles at least get it right
a;							// -0

// but the spec insists on lying to you!
a.toString();				// "0"
a + "";						// "0"
String( a );				// "0"

// strangely, even JSON gets in on the deception
JSON.stringify( a );		// "0"
```

有趣的是，方向操作（从字符串转为数字）可以正常转换：

```js
+"-0";				// -0
Number( "-0" );		// -0
JSON.parse( "-0" );	// -0
```

**警告：**当你发现`JSON.stringify( -0 )`的结果是`"0"`，而与它相反的操作，`JSON.parse( "-0" )`的结果返回`-0`（正如你说预期的那样），你会感觉特别奇怪。（原句：**Warning:** The `JSON.stringify( -0 )` behavior of `"0"` is particularly strange when you observe that it's inconsistent with the reverse: `JSON.parse( "-0" )` reports `-0` as you'd correctly expect.）

除了字符串转换会隐藏负零的真实值（来欺骗你），比较运算符也会欺骗你。

```js
var a = 0;
var b = 0 / -3;

a == b;		// true
-0 == 0;	// true

a === b;	// true
-0 === 0;	// true

0 > -0;		// false
a > b;		// false
```

很显然，如果你想在你的代码中区分`-0`和`0`，你肯定不能相信那坑爹的开发者控制台的输出，你必须变得更机智点：

```js
function isNegZero(n) {
	n = Number( n );
	return (n === 0) && (1 / n === -Infinity);
}

isNegZero( -0 );		// true
isNegZero( 0 / -3 );	// true
isNegZero( 0 );			// false
```

除了无聊的学术需要，我们为什么需要负零？

这里有一些应用，开发者用一个值的幅度来代表一些信息（例如，每帧动画移动的速度），并且数字的符号代表另一条信息（例如，移动的方向）。

在这些应用中，举个例子，如果一个变量变为零并且失去了它的符号信息，那么你也将失去它是从哪个方向到达零这条信息。保留零的符号能够防止潜在的有用信息丢失。

### Special Equality

As we saw above, the `NaN` value and the `-0` value have special behavior when it comes to equality comparison. `NaN` is never equal to itself, so you have to use ES6's `Number.isNaN(..)` (or a polyfill). Simlarly, `-0` lies and pretends that it's equal (even `===` strict equal -- see Chapter 4) to regular positive `0`, so you have to use the somewhat hackish `isNegZero(..)` utility we suggested above.

As of ES6, there's a new utility that can be used to test two values for absolute equality, without any of these exceptions. It's called `Object.is(..)`:

```js
var a = 2 / "foo";
var b = -3 * 0;

Object.is( a, NaN );	// true
Object.is( b, -0 );		// true

Object.is( b, 0 );		// false
```

There's a pretty simple polyfill for `Object.is(..)` for pre-ES6 environments:

```js
if (!Object.is) {
	Object.is = function(v1, v2) {
		// test for `-0`
		if (v1 === 0 && v2 === 0) {
			return 1 / v1 === 1 / v2;
		}
		// test for `NaN`
		if (v1 !== v1) {
			return v2 !== v2;
		}
		// everything else
		return v1 === v2;
	};
}
```

`Object.is(..)` probably shouldn't be used in cases where `==` or `===` are known to be *safe* (see Chapter 4 "Coercion"), as the operators are likely much more efficient and certainly are more idiomatic/common. `Object.is(..)` is mostly for these special cases of equality.

## Value vs. Reference

In many other languages, values can either be assigned/passed by value-copy or by reference-copy depending on the syntax you use.

For example, in C++ if you want to pass a `number` variable into a function and have that variable's value updated, you can declare the function parameter like `int& myNum`, and when you pass in a variable like `x`, `myNum` will be a **reference to `x`**; references are like a special form of pointers, where you obtain a pointer to another variable (like an *alias*). If you don't declare a reference parameter, the value passed in will *always* be copied, even if it's a complex object.

In JavaScript, there are no pointers, and references work a bit differently. You cannot have a reference from one JS variable to another variable. That's just not possible.

A reference in JS points at a (shared) **value**, so if you have 10 different references, they are all always distinct references to a single shared value; **none of them are references/pointers to each other.**

Moreover, in JavaScript, there are no syntactic hints that control value vs. reference assignment/passing. Instead, the *type* of the value *solely* controls whether that value will be assigned by value-copy or by reference-copy.

Let's illustrate:

```js
var a = 2;
var b = a; // `b` is always a copy of the value in `a`
b++;
a; // 2
b; // 3

var c = [1,2,3];
var d = c; // `d` is a reference to the shared `[1,2,3]` value
d.push( 4 );
c; // [1,2,3,4]
d; // [1,2,3,4]
```

Simple values (aka scalar primitives) are *always* assigned/passed by value-copy: `null`, `undefined`, `string`, `number`, `boolean`, and ES6's `symbol`.

Compound values -- `object`s (including `array`s, and all boxed object wrappers -- see Chapter 3) and `function`s -- *always* create a copy of the reference on assignment or passing.

In the above snippet, because `2` is a scalar primitive, `a` holds one initial copy of that value, and `b` is assigned another *copy* of the value. When changing `b`, you are in no way changing the value in `a`.

But **both `c` and `d`** are separate references to the same shared value `[1,2,3]`, which is a compound value. It's important to note that neither `c` nor `d` more "owns" the `[1,2,3]` value -- both are just equal peer references to the value. So, when using either reference to modify (`.push(4)`) the actual shared `array` value itself, it's affecting just the one shared value, and both references will reference the newly modified value `[1,2,3,4]`.

Since references point to the values themselves and not to the variables, you cannot use one reference to change where another reference is pointed:

```js
var a = [1,2,3];
var b = a;
a; // [1,2,3]
b; // [1,2,3]

// later
b = [4,5,6];
a; // [1,2,3]
b; // [4,5,6]
```

When we make the assignment `b = [4,5,6]`, we are doing absolutely nothing to affect *where* `a` is still referencing (`[1,2,3]`). To do that, `b` would have to be a pointer to `a` rather than a reference to the `array` -- but no such capability exists in JS!

The most common way such confusion happens is with function parameters:

```js
function foo(x) {
	x.push( 4 );
	x; // [1,2,3,4]

	// later
	x = [4,5,6];
	x.push( 7 );
	x; // [4,5,6,7]
}

var a = [1,2,3];

foo( a );

a; // [1,2,3,4]  not  [4,5,6,7]
```

When we pass in the argument `a`, it assigns a copy of the `a` reference to `x`. `x` and `a` are separate references pointing at the same `[1,2,3]` value. Now, inside the function, we can use that reference to mutate the value itself (`push(4)`). But when we make the assignment `x = [4,5,6]`, this is in no way affecting where the initial reference `a` is pointing -- still points at the (now modified) `[1,2,3,4]` value.

There is no way to use the `x` reference to change where `a` is pointing. We could only modify the contents of the shared value that both `a` and `x` are pointing to.

To accomplish changing `a` to have the `[4,5,6,7]` value contents, you can't create a new `array` and assign -- you must modify the existing `array` value:

```js
function foo(x) {
	x.push( 4 );
	x; // [1,2,3,4]

	// later
	x.length = 0; // empty existing array in-place
	x.push( 4, 5, 6, 7 );
	x; // [4,5,6,7]
}

var a = [1,2,3];

foo( a );

a; // [4,5,6,7]  not  [1,2,3,4]
```

As you can see, `x.length = 0` and `x.push(4,5,6,7)` were not creating a new `array`, but modifying the existing shared `array`. So of course, `a` references the new `[4,5,6,7]` contents.

Remember: you cannot directly control/override value-copy vs. reference -- those semantics are controlled entirely by the type of the underlying value.

To effectively pass a compound value (like an `array`) by value-copy, you need to manually make a copy of it, so that the reference passed doesn't still point to the original. For example:

```js
foo( a.slice() );
```

`slice(..)` with no parameters by default makes an entirely new (shallow) copy of the `array`. So, we pass in a reference only to the copied `array`, and thus `foo(..)` cannot affect the contents of `a`.

To do the reverse -- pass a scalar primitive value in a way where its value updates can be seen, kinda like a reference -- you have to wrap the value in another compound value (`object`, `array`, etc) that *can* be passed by reference-copy:

```js
function foo(wrapper) {
	wrapper.a = 42;
}

var obj = {
	a: 2
};

foo( obj );

obj.a; // 42
```

Here, `obj` acts as a wrapper for the scalar primitive property `a`. When passed to `foo(..)`, a copy of the `obj` reference is passed in and set to the `wrapper` parameter. We now can use the `wrapper` reference to access the shared object, and update its property. After the function finishes, `obj.a` will see the updated value `42`.

It may occur to you that if you wanted to pass in a reference to a scalar primitive value like `2`, you could just box the value in its `Number` object wrapper (see Chapter 3).

It *is* true a copy of the reference to this `Number` object *will* be passed to the function, but unfortunately, having a reference to the shared object is not going to give you the ability to modify the shared primitive value, like you may expect:

```js
function foo(x) {
	x = x + 1;
	x; // 3
}

var a = 2;
var b = new Number( a ); // or equivalently `Object(a)`

foo( b );
console.log( b ); // 2, not 3
```

The problem is that the underlying scalar primitive value is *not mutable* (same goes for `String` and `Boolean`). If a `Number` object holds the scalar primitive value `2`, that exact `Number` object can never be changed to hold another value; you can only create a whole new `Number` object with a different value.

When `x` is used in the expression `x + 1`, the underlying scalar primitive value `2` is unboxed (extracted) from the `Number` object automatically, so the line `x = x + 1` very subtly changes `x` from being a shared reference to the `Number` object, to just holding the scalar primitive value `3` as a result of the addition operation `2 + 1`. Therefore, `b` on the outside still references the original unmodified/immutable `Number` object holding the value `2`.

You *can* add properties on top of the `Number` object (just not change its inner primitive value), so you could exchange information indirectly via those additional properties.

This is not all that common, however; it probably would not be considered a good practice by most developers.

Instead of using the wrapper object `Number` in this way, it's probably much better to use the manual object wrapper (`obj`) approach in the earlier snippet. That's not to say that there's no clever uses for the boxed object wrappers like `Number` -- just that you should probably prefer the scalar primitive value form in most cases.

References are quite powerful, but sometimes they get in your way, and sometimes you need them where they don't exist. The only control you have over reference vs. value-copy behavior is the type of the value itself, so you must indirectly influence the assignment/passing behavior by which value types you choose to use.

## Review

In JavaScript, `array`s are simply numerically indexed collections of any value-type. `string`s are somewhat "`array`-like", but they have distinct behaviors and care must be taken if you want to treat them as `array`s. Numbers in JavaScript include both "integers" and floating-point values.

Several special values are defined within the primitive types.

The `null` type has just one value: `null`, and likewise the `undefined` type has just the `undefined` value. `undefined` is basically the default value in any variable or property if no other value is present. The `void` operator lets you create the `undefined` value from any other value.

`number`s include several special values, like `NaN` (supposedly "Not a Number", but really more appropriately "invalid number"); `+Infinity` and `-Infinity`; and `-0`.

Simple scalar primitives (`string`s, `number`s, etc.) are assigned/passed by value-copy, but compound values (`object`s, etc.) are assigned/passed by reference-copy. References are not like references/pointers in other languages -- they're never pointed at other variables/references, only at the underlying values.
