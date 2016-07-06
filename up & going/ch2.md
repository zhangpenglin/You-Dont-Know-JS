# 你不知道的JavaScript: Up & Going
# 第二章: 走进JavaScript

在前面的章节中，我介绍了编程的基本构建模块，如变量、循环、条件语句和函数。当然，所有的示例代码都是用JavaScript写的。但是在本章，我们会特别关注作为一个JS开发者你需要了解JavaScript的特性。

在本章中，我们将介绍一些概念，但是不会充分挖掘，我们会在后续的**YDKJS**中详细深入的讲解。你可以将本章看作是介绍整个系列其他话题的概述。

特别是如果你刚学习JavaScript，你更应该多花点时间多次回顾下这些概念和示例代码。任何良好的基础都是一点一点搭建起来的，所以不要指望一开始你就能立刻掌握所有的东西。

你深入学习JavaScript的旅程将从这里开始。

**注意：**正如我在第一章所说的，当你阅读学习本章的时候，你一定要自己尝试所有的代码。你要明白这里有些代码假定你的JavaScript有某些新版本的功能，我写作本书的时候用的是最新的JavaScript版本（第6版的ECMAScript，通常被称为“ES6”——JS的官方规范）。如果你恰好在使用老版本的，ES6版本前的浏览器，这些代码可能没法工作。你应该更新你当前的浏览器（如Chrome，Firefox或IE）到最新版本。

## 值和类型

正如我们在第1章所断言的，JavaScript的值是有类型的，但变量是没有类型的。以下是JavaScript的内置类型：

* `string`
* `number`
* `boolean`
* `null` and `undefined`
* `object`
* `symbol` (new to ES6)

JavaScript提供`typeof`运算符来检测值的类型：

```js
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug

a = undefined;
typeof a;				// "undefined"

a = { b: "c" };
typeof a;				// "object"
```

`typeof`运算符的返回结果始终是六个中的某个（在ES6中是七种——“symbol”类型）字符串值。意思是`typeof "abc"`返回结果是`"string"`而不是`string`。

请注意，在这个代码片段中，变量`a`存储了不同类型的值，揭去表面，你会发现`typeof a`并不是询问变量`a`的类型，而是询问变量`a`当前存储的值的类型是什么。记住，在JavaScript中，只有值才有类型；变量只是存储值的容器。

`typeof null`是个有趣的案例，你期望它返回`"null"`，它却给你错误的返回了`"object"`。

**警告：**这是JS由来已久的错误，而且是一个可能永远不会被修复的错误。在Web网站上有太多的代码依赖于这个错误，如果修正这个错误会导致更多的其他错误！

此外，还要注意`a = undefined`。我们明确设置变量`a`的值为`undefined`，但是这种行为和不给变量赋值没有任何区别，就像上面代码的第一行`var a;`。可以通过不同的方式给一个变量赋值为“undefined”，包括没有返回值的函数调用和使用运算符`void`。

### 对象

`object`类型是一个复合值，在这里你可以给它设置属性，每个属性都持有它们自己的任何类型的值。这或许是所有JavaScript中最有用的值类型之一。

```js
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
```

来看看下面这个`obj`的值（这样看起来似乎更好理解）：

<img src="fig4.png">

属性可以通过**点符号**（即`obj.a`）或者**括号符号**（即`obj["a"]`）来访问其值。点符号更短，并且更容易阅读，因此优选选择使用点符号（如果条件允许的话）。

括号符号会变得很有用当你想要访问的属性名称中含有特殊字符，比如`obj["hello world"]`——这种通过括号来访问的属性被称为**键**。`[]`符号需要一个变量（稍后解释）或者一个`string`（字符串）字面量（需要被包裹在`".."`或`'..'`中）。

当然，当你想要访问的属性或键的名称值，存放在另外一个变量中，括号符号变得很有作用：

```js
var obj = {
	a: "hello world",
	b: 42
};

var b = "a";

obj[b];			// "hello world"
obj["b"];		// 42
```

**注意：**关于JavaScript对象的更多信息，请参阅本系列标题为“**this和对象原型**”的相关内容，特别是第三章。

在JavaScript程序中，还有一些其他的值类型经常会用到：`array`（数组）和`function`（函数）。但是它们不是标准的内置类型，更像是子类型——特殊版本的`object`（对象）类型。

#### 数组

数组是一个`object`持有的值（可以是任何类型）没有特别的命名或键，而是数字索引位置。例如：

```js
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```

**注意：**那些从0开始计数的编程语言，比如JS，使用下标`0`作为数组的第一个元素。

用图形化的形式来思考`arr`可能会比较有帮助：

<img src="fig5.png">

因为数组是特殊的对象（正如`typeof`所展示的那样），它也能有自己的属性，包括那个自动更新的`length`属性。

理论上来说，你可以把数组当成一个正常的对象，给它设置你自己命名的属性，或者你也可以使用`object`对象，但是类似于数组一样只给它设置数字属性（`0`、`1`等）。然而，这通常被认为是各类型的不当使用。

使用数组最好并且最自然的方法就是使用数字下标的值，而使用`object`（对象）来表示命名属性。

#### 函数

在JS程序中，你会一直用的另外一个`object`子类型是`function`函数：

```js
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

再说一遍，函数是`object`的子类型——尽管`typeof`返回`"function"`，这说明`function`（函数）也是一个主要类型——因此它可以有属性，但是你通常只会在有限的情况下使用函数对象的属性（就像`foo.bar`）。

**注意：**有关JS值和类型的详细信息，请参阅本系列标题为**类型和语法**的前两章。

### 内置类型方法

我们刚刚讨论的内置类型和子类型暴露了一些非常强大和有用的行为（方法）作为它们的属性。

如下例子：

```js
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```

理解调用`a.toUpperCase()`的背后是如何实现的比认为这个方法就是存在这个值中更加复杂。（原句：The "how" behind being able to call `a.toUpperCase()` is more complicated than just that method existing on the value.）

简单来说，这里有个`String`（注意，首字母是大写的`S`）对象包装的形式，通常称为“本地类型”，它包含了一个`string`类型的属性；正是这个对象包装类在它的原型上定义了方法`toUpperCase()`。（原句：Briefly, there is a `String` (capital `S`) object wrapper form, typically called a "native," that pairs with the primitive `string` type; it's this object wrapper that defines the `toUpperCase()` method on its prototype.）

当你使用像`"hello world"`这样的原始值来引用一个属性或者方法（如上面示例代码中的`a.toUpperCase()`），JS会自动为这个值进行“装箱”（这是隐式发生的），转换成它对应的包装类。

`string`值会被包裹成`String`对象，`number`值会被包裹成`Number`对象，并且`boolean`值会被包裹成`Boolean`对象。在大多数情况下，你都没有必要担心和使用这些包装对象，你只需使用它们的原始值形式，剩余的事情交给JavaScript就行了。

**注意：**有关JS本地类型和“装箱”，请参阅本系列标题为“**类型和语法**”的第三章。想要更好地理解对象原型，请参阅本系列标题为“**this和对象原型**”的第五章。

### 比较值

在JS程序中，有两种主要的值比较方案：**相等**和**不相等**。任何一种的比较结果都是一个严格的`boolean`值（`true`或`false`），不管比较的值类型是什么。

#### 强制转换

我们在第一章简单的讲解了**强制转换**，让我们在这里重温它。

在JavaScript中有两种形式的强制转换：**显式转换**和**隐式转换**。显式转换很简单，你从代码中就能很明显的看出从一种类型转换到另外一种类型，而隐式转换则发生在一些操作上，这些操作会产生一些非显而易见的副作用。

你可能听别人抱怨过“强制转换是邪恶的”，事实上有例子表明强制转换有时能产生一些令人惊讶的结果。可能没有什么能比编程语言让开发者惊讶更受挫的事情了。

强制转换不是邪恶的，也没有到令人惊讶。事实上，在多数情况下，你使用类型转换是相当明智且易于理解的，甚至可以用来**改善**你代码的可读性。但是我不打算在这里作深入讲解——在本系列标题为“**类型和语法**”的第四章涵盖了转换的所有内容。

这里是一个**显式转换**的例子：

```js
var a = "42";

var b = Number( a );

a;				// "42"
b;				// 42 -- the number!
```

这里是一个**隐式转换**的例子：

```js
var a = "42";

var b = a * 1;	// "42" implicitly coerced to 42 here

a;				// "42"
b;				// 42 -- the number!
```

#### 真与假

在第一章中，我们简要的提到了表示“真”和“假”的一些自然值：当非`boolean`值被强制转为`boolean`，它们会被分别转为`true`或`false`。

下面这些值在JavaScript中表示“假”：

* `""` (empty string)
* `0`, `-0`, `NaN` (invalid `number`)
* `null`, `undefined`
* `false`

不在“假”值名单中的其他任何值都是“真”的。这里是一些例子：

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)

重要的是要记住，只要非`boolean`的值转换成`boolean`值，就一定会遵守上面那个“真”和“假”的列表。下次当你碰到看似一个值被转为`boolean`时实际上却没有时，你就一点都不会感到迷惑了。（原句：It's important to remember that a non-`boolean` value only follows this "truthy"/"falsy" coercion if it's actually coerced to a `boolean`. It's not all that difficult to confuse yourself with a situation that seems like it's coercing a value to a `boolean` when it's not.）

#### 相等

这里有四个相等运算符：`==`、`===`、`!=`和`!==`。`!`表示相等版本的对立面（不相等）；**不相等**（**non-equality**）不应该与**不等**（**inequality**）混为一谈。

`==`和`===`的不同之处在于`==`检测值是否相等而`===`检测值和值的类型是否相等。然而，这么说是不准确的。准确来说，`==`检测值是否相等的时候允许发生转换（隐式转换），而`===`检测值的时候则不允许发生转换；这就是为什么`===`被称为“严格相等”的原因。

考虑如下`==`非严格相等中发生的隐式转换和`===`严格相等：

```js
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false
```

在比较`a == b`时，JS发现二者类型不匹配，因此经过一系列有序的步骤，将其中一个或者两个值转换成不同的类型，直到二者类型匹配，这时就可以进行简单的值比较了。

如果你仔细想想，会发现这里有两种可能的转换方式可以使`a == b`返回`true`。可能最终的比较是`42 == 42`或者是`"42" == "42"`。所以到底是哪个呢？

答案是`"42"`变成了`42`，最终的比较是`42 == 42`。在这个简单的例子中，最终的比较是什么形式的似乎变得无关紧要，反正最终的比较结果是一样的。还有更复杂的情况，真正重要的不是最终的比较结果，而是你知道**如何**得到这个比较结果。（原句：There are more complex cases where it matters not just what the end result of the comparison is, but *how* you get there.）

`a === b`的结果是`false`，因为不允许发生强制转换，因此简单的值比较就会失败。许多的开发人员认为`===`的结果是更可预测的，所以他们主张始终使用这种形式，尽量避免使用`==`。我认为这种观点是非常短视的。我相信`==`是一个强大的工具，可以帮助的程序，**只要你花费时间去学习它是如何工作的**。

我不打算在这里讲解关于`==`相等比较中转换是如何工作的深入细节。其中很大一部分是相当容易觉察到的，但也有一些重要的边角情况需要额外小心。你可以阅读ES5规范中章节11.9.3，看看具体的规则，当你发现相对于所有周围的负面评论，这个机制是如此的简单，你会感到吃惊的。（http://www.ecma-international.org/ecma-262/5.1）

我帮你把一大堆的细节归纳为几个比较简单的规则，帮助你在各种情况下知道该使用`==`或`===`，下面是我的总结的规则：

* 如果比较的两个值可能是`true`或`false`，避免使用`==`
* 如果比较的两个值是这些特定值（`0`、`""`、`[]`——空数组），避免使用`==`
* **所有**其他情况下，你都可以放心使用`==`。它不仅是安全的，在很多情况下，它可以通过提高可读性来简化你的代码。

该总结什么样子的规则，需要你自己进行批判性的思考你的代码中什么类型的值能够进行相等比较。（原句：What these rules boil down to is requiring you to think critically about your code and about what kinds of values can come through variables that get compared for equality.）如果你对这些值很确定，那使用`==`就是安全的，尽管使用它吧！如果你对这些值不太确定，那就使用`===`。就是这么简单。

`!=`是`==`的不相等版本，`!==`是`===`的不相等版本。我们刚刚讨论的所有规则都适用于不相等比较。

如果你比较两个非基本值，如`object`（包括`function`和`array`），你应该特别注意`==`和`===`的比较规则。因为这些值实际上是引用，而`==`和`===`比较只会检查引用是否相等，而不会检查它们对应的值是否相等。

例如，默认情况下`array`（数组）转为`string`（字符串）是通过简单地把所有值通过逗号（`,`）连接在一起。你可能会认为两个拥有相同内容的`array`是相等的，但是实际上并不是这样的：

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

**注：**有关`==`相等比较规则的详细信息，请参阅ES5规范（第11.9.3）和本系列标题为“**类型和语法**”的第四章；参阅第二章“**值和引用**”去了解更多信息。

#### 不等

`<`、`>`、`<=`和`>=`运算符用于不等比较，就是规范中提到的“**关系比较**”。通常他们会用于比较有顺序的值，比如`number`。很容易理解`3 < 4`。

但是在JavaScript中`string`值也能被用于不等比较，它使用的是字母表规则（`"bar" < "foo"`）。

强制转换？类似`==`比较的规则（尽管它们不完全相同！）引用到不等运算符上。值得注意的是，这里没有“**严格不等**”运算符用来禁止强制转换，就像`===`“**严格相等**”所做的。

考虑如下：

```js
var a = 41;
var b = "42";
var c = "43";

a < b;		// true
b < c;		// true
```

上面比较发生了什么？在ES5规范的第11.8.5中提到，如果进行`<`比较的两个值都是`string`，就像上面的`b < c`，就会按照字典序进行比较（即按照字典里面的字母排列顺序）。但是如果其中一个或者两个都不是`string`，就像上面的`a < b`，那两个值都会被强制转换成`number`，然后进行数值比较。

在不同类型的值比较中，你可能会遇到一个特别坑的情况——记住，这里没有“**严格不等**”的功能——当其中某个值不能被转换成一个有效的数字，例如：

```js
var a = 42;
var b = "foo";

a < b;		// false
a > b;		// false
a == b;		// false
```

等等，怎么可能三个比较都是`false`呢？因为在`<`和`>`比较中`b`被强制转换成“无效的数字”`NaN`，ES规范指出`NaN`既不大于也不小于其他任何值。

`==`比较失败出于不同的原因。`a == b`可能被解释成`42 == NaN`或者`"42" == "foo"`（正如我们前面说提到的），但是结果都是`false`。（事实上，它被解释为`42 == NaN`）

**注意：**关于不等比较规则的详细信息，请参阅ES5规范的第11.8.5章节，也可以参考本系列标题为“**类型和语法**”的第四章。

## 变量

在JavaScript中，变量名（包括函数名）必须是有效的**标识符**。关于有效标识符中严格和完整的规则，当你考虑到非传统字符（如Unicode）的时候有点复杂。但是如果你只考虑ASCII字母数字字符的话，这个规则就很简单了。

标识符必须以`a`-`z`、`A`-`Z`、`$`或`_`开头，第二个字符开始它可以包括任意这些字符和数字`0`-`9`。

一般来说，变量标识符命名规则也适用于属性命名。然而，一些特定的单词不能被用作变量，但是可以用作属性名称。这些单词被称为“保留字”，并且包括JS关键字（`for`、`in`、`if`等）以及`null`、`true`和`false`。

**注意：**有关保留字的信息信息，请参阅本系列标题为“**类型和语法**”的附录A。

### 函数作用域

你可以使用`var`关键字在当前函数作用域或者全局作用域（在任何函数之外的顶层作用域）声明一个变量。

#### 提升

只要`var`声明的变量出现在一个作用域中，该声明属于整个作用域，并且在该作用域任何地方都可以被访问到。

当`var`声明概念上被“**移动**”到封闭作用域的顶部，这个行为称为“提升”。从技术上讲，通过解释代码是如何编译的来解释这个过程会更准确，但是现在我们先跳过这些细节。

考虑如下：

```js
var a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2
```

**警告：**依靠变量**提升**在它的作用域里面较早的使用一个变量（在`var`声明之前出现），这通常并不常见，也不是一个好主意；它通常会让人感到迷惑。但是更普遍接受和使用的是函数声明**提升**，正如我们对`foo()`所做的那样，在正式声明之前使用它。

#### 嵌套作用域

当你声明一个变量，在那个作用域里面任何地方，以及它低/内部作用域，都可以被访问到这个变量。例如：

```js
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b );		// 1 2
	}

	bar();
	console.log( a );				// 1
}

foo();
```

注意到在`bar()`中不能访问到变量`c`，因为它是在内部的`baz()`函数作用域声明的，同理，函数`foo()`也不能访问到变量`b`。

如果你试图访问一个作用域中不存在的变量，程序会抛出`ReferenceError`异常。如果你试图给一个没有被声明过的变量赋值，你最终会在全局（顶级的）作用域（这很糟糕！）创建一个变量或者得到一个错误，这取决于你是否处于“严格模式”（参见“严格模式”）。让我们来看看如下例子：

```js
function foo() {
	a = 1;	// `a` not formally declared
}

foo();
a;			// 1 -- oops, auto global variable :(
```

这是一个非常不好的做法。千万别这么做！（使用你的变量之前）应该始终先声明你的变量。

除了在函数级作用域上创建变量声明，ES6新增加了**块级作用域**（`{ .. }`），通过使用`let`关键字，能够让你声明的变量属于一个单独的代码块。除了一些细致入微的细节，它的作用域规则和我们所看到的函数作用域大致相同：

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

由于使用`let`代替`var`，`b`只属于`if`语句，而不是整个`foo()`的函数作用域。同样，`c`也只属于`while`循环。块作用域在更加细粒度管理变量作用域上非常有用，并且随着时间的推移能使你的代码更易于维护。

**注意：**有关作用域的详细信息，请参阅本系列标题为“**作用域和闭包**”的相关内容。有关`let`块作用域的更多信息，请参阅本系列标题为“**ES6及展望**”的相关内容。

## 条件语句

除了我们在第一章简单介绍的`if`语句，JavaScript还提供了一些其他形式的条件语句，我们来看看。

有时你会发现自己写了一系列`if..else..if`这样的语句：

```js
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```

这种结构虽然可以工作，但是它有点啰嗦，因为你需要为每种情况都测试`a`。下面是另外一种选项，`switch`（开关）语句：

```js
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

如果你只希望在一个`case`语句中运行，那`break`语句就非常重要了。如果省略了`case`中的`break`语句，当这个`case`匹配并且运行完后，会继续执行下一个`case`语句，不管这个`case`是否匹配。这种现象称为“fall through”（落空），有时会有用（但是大多数情况下会让你抓狂）。

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```

在这里，如果`a`的值是`2`或`10`，就会执行“some cool stuff”这段代码。

在JavaScript中另一种形式的条件语句是“条件运算符”，经常被称为“三元运算符”。它更像是一个`if..else`语句的简洁形式，例如：

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

如果测试条件（这里的`a > 41`）的计算结果是`true`，结果就是第一条（`"hello"`），否则的话结果就是第二条（`"world"`），不管结果是什么，最后都会将结果赋值给`b`。

条件运算符可以不被用于赋值，当这绝对是它最常见的用法。

**注意：**有关条件测试和其他形式如`switch`和`? :`的条件语句的更多信息，请参阅本系列标题为“类型和语法”的相关内容。

## 严格模式

ES5为JS添加了“**严格模式**”，它对某些行为的规则进行了限制。通常情况下，这些限制被视为将代码保持在一个更安全和更合适的集合中的一套准则。此外，使用严格模式能够让引擎更好的优化你的代码。严格模式是代码的一大胜利，你应该在你的所有程序中使用严格模式。

你可以选择在一个函数上使用严格模式，或者在整个文件中使用，这取决于你将编译标注放在哪里。

```js
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

与此相比：

```js
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```

使用严格模式与不使用严格模式的一个关键的区别（改善）在于严格模式禁止使用自动全局变量声明：

```js
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

如果你在你的代码中启用严格模式，当你得到错误提示或者代码开始出现bug了，你的本能反应可能关闭严格模式。但是沉浸在本能反应（躲避严格模式）可不是一个好主意。如果严格模式导致了你的程序出问题了，几乎可以肯定的是，你的程序中有问题，并且你应该解决这个问题。

严格模式不仅会让你的代码保持在更安全的道路上，不仅会让你的代码更优化，它还代表了语言的未来发展方向。你现在就适应严格模式会比关闭严格模式更简单——越到后面代码就越难转换。

**注意：**关于严格模式的详细信息，请参阅本系列标题为“类型和语法”的第五章。

## 函数作为值

到目前为止，我们已经讨论了函数**作用域**是JavaScript中的重要机制。你还记得典型的`function`（函数）声明语法如下：

```js
function foo() {
	// ..
}
```

虽然从语法上看可能不明显，但是`foo`只是外层作用域的一个变量，指向刚刚声明的`function`。也就是说，`function`本身就是一个值，就像`42`或`[1,2,3]`一样。

起初听起来像是一个奇怪的概念，所以请花点时间去思考它。你不仅可以传递一个值（参数）**给**一个函数，而且**函数本身可以作为一个值**赋给变量，或者作为参数传递给其他函数以及作为其他函数的返回值。

这样，一个函数值应该被认为是一个表达式，就像任何其他值或表达式。

考虑如下：

```js
var foo = function() {
	// ..
};

var x = function bar(){
	// ..
};
```

分配变量`foo`的第一个函数表达式被称为**匿名**函数，因为它没有`name`（名字）。

第二个函数表达式是**命名**（`bar`）函数，尽管它的引用被赋值给变量`x`。**命名函数表达式**一般比较可取，虽然**匿名函数表达式**仍然是非常普遍的。

想了解更多相关信息，请参阅本系列标题为“**作用域和闭包**”的相关内容。

### 立即执行函数表达式

在之前的代码片段中，没有任何一个函数表达式可以立即执行——除非我们使用`foo()`或`x()`来调用函数。

还有另一种方法来执行一个函数表达式，通常为称为**立即执行函数表达式**（IIFE，**immediately invoked function expression**）：

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

围绕在`(function IIFE(){ .. })`函数表达式外层的`( .. )`是为了防止它被视为普通的函数声明，这是JS语法的细微之处。（原句：The outer `( .. )` that surrounds the `(function IIFE(){ .. })` function expression is just a nuance of JS grammar needed to prevent it from being treated as a normal function declaration.）

在表达式最后的那个`()`——`})();`——是它立即执行之前引用的函数表达式。

这看起来似乎有点奇怪，但第一眼看起来似乎也不是那么陌生。考虑`foo`和`IIFE`之间的相似之处：

```js
function foo() { .. }

// `foo` function reference expression,
// then `()` executes it
foo();

// `IIFE` function expression,
// then `()` executes it
(function IIFE(){ .. })();
```

如你所见，在执行`()`之前的声明`(function IIFE(){ .. })`和执行`()`之前函数`foo`的声明基本上是一样的；在这两种情况下，函数引用加上`()`会被立即执行。（原句：As you can see, listing the `(function IIFE(){ .. })` before its executing `()` is essentially the same as including `foo` before its executing `()`; in both cases, the function reference is executed with `()` immediately after it.）

因为IIFE仅仅只是一个函数，而函数会创建变量**作用域**，IIFE这种函数声明的方式，常用来声明不会影响IIFE外周边代码的变量（原句：using an IIFE in this fashion is often used to declare variables that won't affect the surrounding code outside the IIFE）：

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42
```

IIFEs也可以有返回值：

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

`IIFE`命名函数执行并返回了值`42`，然后该值被赋给变量`x`。

### 闭包

**闭包**是JavaScript中最重要并且最少被理解的的概念。在这里我不会做深入讨论，而是指引你去阅读本系列标题为“作用域和闭包”的相关内容。但是我会在这里介绍一些关于闭包的知识让你对基本概念有所了解。这将是在你的JS技能组中最重要的技术之一。

你可以把闭包理解为一种在函数运行完毕之后“记住”并且继续访问函数作用域（它的变量）的方法。（原句：You can think of closure as a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running.）

考虑如下：

```js
function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}
```

每次调用函数`makeAdder(..)`的时候，内部函数`add(..)`的引用都会被返回，并且会记住传递给`makeAdder(..)`函数的参数`x`的值。现在，让我来使用`makeAdder(..)`：

```js
// `plusOne` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusOne = makeAdder( 1 );

// `plusTen` gets a reference to the inner `add(..)`
// function with closure over the `x` parameter of
// the outer `makeAdder(..)`
var plusTen = makeAdder( 10 );

plusOne( 3 );		// 4  <-- 1 + 3
plusOne( 41 );		// 42 <-- 1 + 41

plusTen( 13 );		// 23 <-- 10 + 13
```

更多关于这些代码是如何工作的：

1. 当我们调用`makeAdder(1)`，我们获得了一个内部函数`add(..)`的引用，并且它记住了变量`x`的值为`1`。我们给这个函数取个别名叫`plusOne(..)`。
2. 当我们调用`makeAdder(10)`，我们获得了一个内部函数`add(..)`的引用，并且它记住了变量`x`的值为`10`。我们给这个函数取个别名叫`plusTen(..)`。
3. 当我们调用`plusOne(3)`，它把`3`（其内部的`y`）加上`1`（被记住的`x`的值），于是我们得到了结果`4`。
4. 当我们调用`plusTen(13)`，它把`13`（其内部的`y`）加上`10`（被记住的`x`的值），于是我们得到了结果`23`。

第一次看起来这似乎有点奇怪并且感到困惑，但是别担心，它就是这样子！你需要大量的实践去充分了解它。

但是请相信我，一旦你掌握了它，这在所有编程中是最强大和最有用的技术之一。闭包绝对值得你付出巨大的努力去掌握它（原句：It's definitely worth the effort to let your brain simmer on closures for a bit.）。在接下来的部分，我们会使用闭包做更多的实践。

#### 模块

闭包在JavaScript中最常见的用法就是模块模式。模块能够让你定义私有实现细节，将变量函数等与外界隔离，以及提供一个能从外界访问的公共API。

考虑如下：

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

函数`User()`提供了一个作用域保持变量`username`和`password`，以及内部函数`doLogin()`；这些都是`User`模块的私有内部细节，从外界不能访问它们。

**警告：**我们在这里故意不调用`new User()`，尽管对大多数读者来说可能更常见。`User()`只是一个函数，不是一个可以实例化的类，所以直接调用是正常的。使用`new`是不合适的，并且浪费资源。

执行`User()`创建了`User`模块的一个**实例**——创建了一个全新的作用域，因此创建了里面每个变量/函数的一个全新副本。我们将这个实例赋值给`fred`。如果我们再一次运行`User()`，我们将得到一个完全独立于`fred`的全新实例。

内部函数`doLogin()`有对`username`和`password`的闭包，这意味着即使函数`User()`运行完毕，`doLogin()`依然保留了对这两个变量的访问。

`publicAPI`是有一个属性/方法的对象，它是内部函数`doLogin()`的一个引用。当我们从`User()`返回`publicAPI`，就成了`fred`的实例。

此时，外部函数`User()`已经执行完毕了。通常情况下，你会觉得像`username`和`password`这样的变量已经消失了。但是实际上并没有，因为在函数`login()`中保留了对它们的引用，所以它们还存活着。

这就是为什么我们调用`fred.login(..)`——相当于调用内部的`doLogin(..)`——时，它仍然可以访问`username`和`password`这两个内部变量。

这是个很好的机会让你开始接触闭包和模块模式，虽然有些知识点仍然让你感到迷惑。没关系！它确实需要你花些时间去仔细理解。

从这里开始，去阅读本系列标题为“作用域和闭包”相关内容，进行更深入的探索。

## this关键字

在JavaScript中另一个非常普通误解的概念是`this`标识符。再次说明，在本系列标题为“**this和对象原型**”中有一系列章节会讲解this相关的内容，因此我们只是在这里对这个概念做个简单介绍。

虽然`this`可能常常看起来跟“面向对象的模式”相关，但是在JS中`this`是一个不同的机制。

如果一个函数内部有`this`引用，那个`this`引用通常会指向一个`object`（对象）。但是指向哪个`object`取决于函数的调用方式。

认识到`this`**不指向**函数本身这点是非常重要的，因为这是最常见的误解。

这里有一个快速说明：

```js
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );	// "obj2"
new foo();			// undefined
```

这里有四条关于`this`是如何设置的规则，并且在上面代码片段的最后四行显示出来了。

1. 在非严格模式下，`foo()`最终将`this`绑定在全局对象上，因此通过`this.bar`得到的值是`"global"`；在严格模式下，`this`是`undefined`，因此在你访问属性`bar`的时候会得到一个错误。
2. `obj1.foo()`将`this`绑定在`obj1`对象上。
3. `foo.call(obj2)`将`this`绑定在`obj2`对象上。
4. `new foo()`将`this`绑定到一个全新的空对象上。

要明白`this`指向谁，你必须明白这些函数是如何被调用的。它肯定是上面显示的四种方法之一，然后再回答`this`是什么。（原句：Bottom line: to understand what `this` points to, you have to examine how the function in question was called. It will be one of those four ways just shown, and that will then answer what `this` is.）

**注意：**有关`this`的更多信息，请参阅本系列标题为“**this和对象原型**”的第一章和第二章。

## Prototypes

The prototype mechanism in JavaScript is quite complicated. We will only glance at it here. You will want to spend plenty of time reviewing Chapters 4-6 of the *this & Object Prototypes* title of this series for all the details.

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on. You could think of this almost as a fallback if the property is missing.

The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called `Object.create(..)`.

Consider:

```js
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

It may help to visualize the `foo` and `bar` objects and their relationship:

<img src="fig6.png">

The `a` property doesn't actually exist on the `bar` object, but because `bar` is prototype-linked to `foo`, JavaScript automatically falls back to looking for `a` on the `foo` object, where it's found.

This linkage may seem like a strange feature of the language. The most common way this feature is used -- and I would argue, abused -- is to try to emulate/fake a "class" mechanism with "inheritance."

But a more natural way of applying prototypes is a pattern called "behavior delegation," where you intentionally design your linked objects to be able to *delegate* from one to the other for parts of the needed behavior.

**Note:** For more information about prototypes and behavior delegation, see Chapters 4-6 of the *this & Object Prototypes* title of this series.

## Old & New

Some of the JS features we've already covered, and certainly many of the features covered in the rest of this series, are newer additions and will not necessarily be available in older browsers. In fact, some of the newest features in the specification aren't even implemented in any stable browsers yet.

So, what do you do with the new stuff? Do you just have to wait around for years or decades for all the old browsers to fade into obscurity?

That's how many people think about the situation, but it's really not a healthy approach to JS.

There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

### Polyfilling

The word "polyfill" is an invented term (by Remy Sharp) (https://remysharp.com/2010/10/08/what-is-a-polyfill) used to refer to taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

For example, ES6 defines a utility called `Number.isNaN(..)` to provide an accurate non-buggy check for `NaN` values, deprecating the original `isNaN(..)` utility. But it's easy to polyfill that utility so that you can start using it in your code regardless of whether the end user is in an ES6 browser or not.

Consider:

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

The `if` statement guards against applying the polyfill definition in ES6 browsers where it will already exist. If it's not already present, we define `Number.isNaN(..)`.

**Note:** The check we do here takes advantage of a quirk with `NaN` values, which is that they're the only value in the whole language that is not equal to itself. So the `NaN` value is the only one that would make `x !== x` be `true`.

Not all new features are fully polyfillable. Sometimes most of the behavior can be polyfilled, but there are still small deviations. You should be really, really careful in implementing a polyfill yourself, to make sure you are adhering to the specification as strictly as possible.

Or better yet, use an already vetted set of polyfills that you can trust, such as those provided by ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim).

### Transpiling

There's no way to polyfill new syntax that has been added to the language. The new syntax would throw an error in the old JS engine as unrecognized/invalid.

So the better option is to use a tool that converts your newer code into older code equivalents. This process is commonly called "transpiling," a term for transforming + compiling.

Essentially, your source code is authored in the new syntax form, but what you deploy to the browser is the transpiled code in old syntax form. You typically insert the transpiler into your build process, similar to your code linter or your minifier.

You might wonder why you'd go to the trouble to write new syntax only to have it transpiled away to older code -- why not just write the older code directly?

There are several important reasons you should care about transpiling:

* The new syntax added to the language is designed to make your code more readable and maintainable. The older equivalents are often much more convoluted. You should prefer writing newer and cleaner syntax, not only for yourself but for all other members of the development team.
* If you transpile only for older browsers, but serve the new syntax to the newest browsers, you get to take advantage of browser performance optimizations with the new syntax. This also lets browser makers have more real-world code to test their implementations and optimizations on.
* Using the new syntax earlier allows it to be tested more robustly in the real world, which provides earlier feedback to the JavaScript committee (TC39). If issues are found early enough, they can be changed/fixed before those language design mistakes become permanent.

Here's a quick example of transpiling. ES6 adds a feature called "default parameter values." It looks like this:

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

Simple, right? Helpful, too! But it's new syntax that's invalid in pre-ES6 engines. So what will a transpiler do with that code to make it run in older environments?

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

As you can see, it checks to see if the `arguments[0]` value is `void 0` (aka `undefined`), and if so provides the `2` default value; otherwise, it assigns whatever was passed.

In addition to being able to now use the nicer syntax even in older browsers, looking at the transpiled code actually explains the intended behavior more clearly.

You may not have realized just from looking at the ES6 version that `undefined` is the only value that can't get explicitly passed in for a default-value parameter, but the transpiled code makes that much more clear.

The last important detail to emphasize about transpilers is that they should now be thought of as a standard part of the JS development ecosystem and process. JS is going to continue to evolve, much more quickly than before, so every few months new syntax and new features will be added.

If you use a transpiler by default, you'll always be able to make that switch to newer syntax whenever you find it useful, rather than always waiting for years for today's browsers to phase out.

There are quite a few great transpilers for you to choose from. Here are some good options at the time of this writing:

* Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
* Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5

## Non-JavaScript

So far, the only things we've covered are in the JS language itself. The reality is that most JS is written to run in and interact with environments like browsers. A good chunk of the stuff that you write in your code is, strictly speaking, not directly controlled by JavaScript. That probably sounds a little strange.

The most common non-JavaScript JavaScript you'll encounter is the DOM API. For example:

```js
var el = document.getElementById( "foo" );
```

The `document` variable exists as a global variable when your code is running in a browser. It's not provided by the JS engine, nor is it particularly controlled by the JavaScript specification. It takes the form of something that looks an awful lot like a normal JS `object`, but it's not really exactly that. It's a special `object,` often called a "host object."

Moreover, the `getElementById(..)` method on `document` looks like a normal JS function, but it's just a thinly exposed interface to a built-in method provided by the DOM from your browser. In some (newer-generation) browsers, this layer may also be in JS, but traditionally the DOM and its behavior is implemented in something more like C/C++.

Another example is with input/output (I/O).

Everyone's favorite `alert(..)` pops up a message box in the user's browser window. `alert(..)` is provided to your JS program by the browser, not by the JS engine itself. The call you make sends the message to the browser internals and it handles drawing and displaying the message box.

The same goes with `console.log(..)`; your browser provides such mechanisms and hooks them up to the developer tools.

This book, and this whole series, focuses on JavaScript the language. That's why you don't see any substantial coverage of these non-JavaScript JavaScript mechanisms. Nevertheless, you need to be aware of them, as they'll be in every JS program you write!

## 总结

The first step to learning JavaScript's flavor of programming is to get a basic understanding of its core mechanisms like values, types, function closures, `this`, and prototypes.

Of course, each of these topics deserves much greater coverage than you've seen here, but that's why they have chapters and books dedicated to them throughout the rest of this series. After you feel pretty comfortable with the concepts and code samples in this chapter, the rest of the series awaits you to really dig in and get to know the language deeply.

The final chapter of this book will briefly summarize each of the other titles in the series and the other concepts they cover besides what we've already explored.
