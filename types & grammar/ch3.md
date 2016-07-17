# 你不知道的JavaScript: 类型和语法
# 第三章：Natives

我们在第一章和第二章中提到过几次built-ins（内置插件），通常称为“natives”，比如`String`和`Number`。现在让我们仔细研究它们。

以下是最常用的natives列表：

* `String()`
* `Number()`
* `Boolean()`
* `Array()`
* `Object()`
* `Function()`
* `RegExp()`
* `Date()`
* `Error()`
* `Symbol()` -- added in ES6!

正如你所见，这些natives实际上是内置函数。

如果你是从像Java这样的语言转过来学习JS，那JavaScript的`String()`看起来像你用来创建字符串值的`String(..)`构造函数。因此，你很快就会看到，你可以做这样的事情：

```js
var s = new String( "Hello World!" );

console.log( s.toString() ); // "Hello World!"
```

这些natives确实可以被用作native构造函数。但是，构造出什么可能跟你想象中的不同。

```js
var a = new String( "abc" );

typeof a; // "object" ... not "String"

a instanceof String; // true

Object.prototype.toString.call( a ); // "[object String]"
```

通过构造函数的形式（`new String("abc")`）创建的值是一个原始类型（`"abc"`）值的对象包裹器。

重要的是，`typeof`显示这些对象的类型并不是它们自己的特殊**类型**（更恰当地说是`object`的子类型）。

我们可以进一步观察这个对象包裹器：

```js
console.log( a );
```

这条语句的输出取决于你的浏览器，开发者控制台选择他们认为合适的做法去序列化对象，然后呈现给开发者。（原句：The output of that statement varies depending on your browser, as developer consoles are free to choose however they feel it's appropriate to serialize the object for developer inspection.）

**注意：**在写这篇文章的时候，最新的Chrome打印是这样的：`String {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}`。但是，旧版本的Chrome以前只打印：`String {0: "a", 1: "b", 2: "c"}`。最新的Firefox目前打印`String ["a","b","c"]`，但是以前以斜体打印`"abc"`，它是可点击的，点击之后打开对象检查器。当然，这些结果都会受到迅速的变化，因此你看到的可能不一样。

问题的关键是，`new String("abc")`在字符串`"abc"`周围创建了一个字符串包装对象，而不仅仅是原始值`"abc"`本身。

## Internal `[[Class]]`

Values that are `typeof` `"object"` (such as an array) are additionally tagged with an internal `[[Class]]` property (think of this more as an internal *class*ification rather than related to classes from traditional class-oriented coding). This property cannot be accessed directly, but can generally be revealed indirectly by borrowing the default `Object.prototype.toString(..)` method called against the value. For example:

`typeof` `"object"`（例如数组）的值会被附加上一个内部`[[Class]]`属性（这更像是面向对象编程中的内部类）。这个属性不能被直接访问到，你可以通过借用默认的`Object.prototype.toString(..)`方法间接的获取这个值。例如：

```js
Object.prototype.toString.call( [1,2,3] );			// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );	// "[object RegExp]"
```

在这个例子中，数组的内部`[[Class]]`的值是`"Array"`，而正则表达式则是`"RegExp"`。在大多数情况下，内部`[[Class]]`的值和内置的native构造函数（见下文）是相对应的，但事实并非总是如此。

对于原始类型的值，情况是什么样子？首先来看`null`和`undefined`：

```js
Object.prototype.toString.call( null );			// "[object Null]"
Object.prototype.toString.call( undefined );	// "[object Undefined]"
```

你可能会发现这里并没有`Null()`或`Undefined()`的native构造函数，但`"Null"`和`"Undefined"`仍然出现在内部`[[Class]]`暴露的值中。

但是对于其他简单的原始类型如`string`、`number`和`boolean`，这里需要提一个概念，就是我们通常所说的“装箱”（参见下一节的“Boxing Wrappers”）：

```js
Object.prototype.toString.call( "abc" );	// "[object String]"
Object.prototype.toString.call( 42 );		// "[object Number]"
Object.prototype.toString.call( true );		// "[object Boolean]"
```

在这段代码中，每个简单的原始类型都被各自对应的封装类自动装箱了，这就是为什么`"String"`、`"Number"`和`"Boolean"`会出现在内部`[[Class]]`的当中的原因。

**注意：**从ES5到ES6的过程中，`toString()`和`[[Class]]`的行为已经发生了一些变化，我们会在本系列标题为“超越ES6”中讲解这些细节。

## Boxing Wrappers

这些封装对象有一个非常重要的目的。原始值是没有属性或方法，所以你需要在原始值的基础上封装一个对象，才能访问`.length`和`.toString()`。幸运的是，JS会自动**装箱**（又称包裹）原始值来实现这样的访问。

```js
var a = "abc";

a.length; // 3
a.toUpperCase(); // "ABC"
```

所以，如果你打算要经常访问你的字符串值的属性或方法，如在`for`循环中的判断条件`i < a.length`，看起来似乎很有理由一开始就把这个值转为对象的形式，这样JS引擎就不用隐式的为我们创建它了。

But it turns out that's a bad idea. Browsers long ago performance-optimized the common cases like `.length`, which means your program will *actually go slower* if you try to "preoptimize" by directly using the object form (which isn't on the optimized path).

事实证明这是一个坏主意。浏览器会对常见的情况（如`.length`）进行性能优化，这意味着如果你直接使用对象的形式反而会使得你的程序**跑得更慢**（因为这不是最优化路径）。

在一般情况下，基本上没有理由直接使用对象的形式。最好是让装箱操作在必要时隐式的发生。换句话说，千万不要做类似`new String("abc")`、`new Number(42)`等这样的事情——请总是使用它们的字面原始值`"abc"`和`42`。

### 封装对象的陷阱

There are some gotchas with using the object wrappers directly that you should be aware of if you *do* choose to ever use them.

如果你**确实**选择使用封装对象，你应该知道这里有一些陷阱与直接使用封装对象有关。

例如，考虑`Boolean`封装值：

```js
var a = new Boolean( false );

if (!a) {
	console.log( "Oops" ); // never runs
}
```

问题就出在你在`false`值周围创建的封装对象上，因为对象是“truthy”（参见第四章），因此使用对象的行为与底层`false`值本身相对立，这与正常的预期相反。

如果你想手动装箱一个原始值，你可以使用`Object(..)`函数（没有`new`关键字）：

```js
var a = "abc";
var b = new String( a );
var c = Object( a );

typeof a; // "string"
typeof b; // "object"
typeof c; // "object"

b instanceof String; // true
c instanceof String; // true

Object.prototype.toString.call( b ); // "[object String]"
Object.prototype.toString.call( c ); // "[object String]"
```

再说一遍，通常不鼓励你直接使用封装类（如上面的`b`和`c`），但是可能在一些非常罕见的情况下你发现它们会很有用。

## Unboxing

如果你有一个封装对象并且想要获取它底层的原始值，你可以使用它的`valueOf()`方法：

```js
var a = new String( "abc" );
var b = new Number( 42 );
var c = new Boolean( true );

a.valueOf(); // "abc"
b.valueOf(); // 42
c.valueOf(); // true
```

当你使用一个封装对象，在某个地方你需要用到这个封装对象的底层原始值，这个时候会隐式的发生拆箱操作。这个过程（强制转换）会在第四章详细介绍，不介意先给你露一手：

```js
var a = new String( "abc" );
var b = a + ""; // `b` has the unboxed primitive value "abc"

typeof a; // "object"
typeof b; // "string"
```

## Natives as Constructors

For `array`, `object`, `function`, and regular-expression values, it's almost universally preferred that you use the literal form for creating the values, but the literal form creates the same sort of object as the constructor form does (that is, there is no nonwrapped value).

对于`array`、`object`、`function`和正则表达式，创建它们的值的普遍首选是字面量形式，但是字面量形式创建的和构造函数形式创建的对象是一样的（也就是说，不存在未封装过的值）。

正如我们上面看到的natives，一般应该避免这些构造函数的形式，除非你真的知道你需要它们，大部分情况下，它们会引入异常和陷阱，你可能真的不希望处理这些。

### `Array(..)`

```js
var a = new Array( 1, 2, 3 );
a; // [1, 2, 3]

var b = [1, 2, 3];
b; // [1, 2, 3]
```

**注意：**`Array(..)`构造函数前面不需要添加`new`关键字。如果你忽略它，它会表现得就像你用了它一样。因此，`Array(1,2,3)`和`new Array(1,2,3)`的结果是相同的。

`Array`构造函数有个特殊的形式，就是如果你只传递一个数字参数，那它不会生成一个含有此数字的数组，而是把这个数字当作数组的初始化长度。

这真是一个可怕的主意。首先，这种形式很容易就把你绊倒，因为它容易忘记。

但更重要的是，你实际上根本就不是初始化数组。相反，你创建的就是一个空数组，并且设置数组的`length`属性为指定的数字。

An array that has no explicit values in its slots, but has a `length` property that *implies* the slots exist, is a weird exotic type of data structure in JS with some very strange and confusing behavior. The capability to create such a value comes purely from old, deprecated, historical functionalities ("array-like objects" like the `arguments` object).

这个数组插槽中没有明确的值，仅仅是属性`length`**暗示**了插槽的存在，在JS中，这是一个怪异奇特的数据结构，带着奇怪和混乱的行为。创建这么奇怪的数组的能力，来自旧的、弃用的历史功能（类数组对象，如`arguments`对象）。

**注意：**数组中至少含有一个“空槽”（empty slot）的数组通常称为“稀疏数组”（sparse array）。

让懵逼来得更猛烈些吧！

我们来看另外一个例子，浏览器的开发者控制台是打印这些对象的结果也不同，这孕育了更多的混乱。

例如：

```js
var a = new Array( 3 );

a.length; // 3
a;
```

在Chrome中（在写作本书的时候）`a`序列化的结果是：`[ undefined x 3 ]`。**这真是不幸。**这暗示有3个`undefined`值在这个数组的插槽中，而实际上根本就不存在插槽（所谓的“空插槽！(empty slots)”——这名字真误导人！）。（译者注：下文中的空插槽，均是指此意。重要的事情再说一遍，空插槽并不是真的是空的插槽，它实际上是不存在的，只是JS传统叫法上叫空插槽，这个叫法十分的误导人，千万别误入歧途！！！）

为了显现差异，试试这个：

```js
var a = new Array( 3 );
var b = [ undefined, undefined, undefined ];
var c = [];
c.length = 3;

a;
b;
c;
```

**注意：**正如你在这个例子中看到的`c`，空插槽可以在数组创建后产生。通过更改数组的`length`让它超过实际上定义的长度，你就隐式的引入了空插槽。事实上，你可以在上面代码中调用`delete b[1]`，它会在`b`的中间引入一个空插槽。

在当前版本的Chrome中，`b`的输出是`[ undefined, undefined, undefined ]`，而`a`和`c`的输出结果是`[ undefined x 3 ]`。一脸懵逼，是吧！没事，其他人跟你一样：）

更糟糕的是，在写作本书的时候，Firefox中`a`和`c`的输出结果是`[ , , , ]`。你看出为什么这么混乱吗？看仔细点。三个逗号意味着四个插槽，而不是我们期望的三个插槽。

**What!?** Firefox puts an extra `,` on the end of their serialization here because as of ES5, trailing commas in lists (array values, property lists, etc.) are allowed (and thus dropped and ignored). So if you were to type in a `[ , , , ]` value into your program or the console, you'd actually get the underlying value that's like `[ , , ]` (that is, an array with three empty slots). This choice, while confusing if reading the developer console, is defended as instead making copy-n-paste behavior accurate.

**我靠！？**Firefox在它的序列化的末尾加上一个额外的`,`，因为在ES5中，在列表（数组值、属性列表等）后面尾随逗号是允许的（会被丢弃或忽略掉）。因此，如果你在你的程序或控制台中输入`[ , , , ]`，实际上得到的值是`[ , , ]`（即，三个空插槽的数组）。这种做法让你在读开发者控制台的输出结果感到困惑，当却被美其名曰这么做能让复制粘贴的行为更准确。

如果你现在一脸懵逼，没啥事，you're not alone！

不幸的是，事情变得更糟糕。不仅仅是开发者控制台的输出让你困惑不解，上面代码中的`a`和`b`在某个情况下表现一致，但在另外一些情况下，表现的**完全不一样**。

```js
a.join( "-" ); // "--"
b.join( "-" ); // "--"

a.map(function(v,i){ return i; }); // [ undefined x 3 ]
b.map(function(v,i){ return i; }); // [ 0, 1, 2 ]
```

**什么鬼！？？？**

`a.map(..)`调用**失败**因为插槽根本就不存在，因此`map(..)`没有任何迭代。`join(..)`的工作方式不同与它。基本上我们想想就知道，可以这么实现它：

```js
function fakeJoin(arr,connector) {
	var str = "";
	for (var i = 0; i < arr.length; i++) {
		if (i > 0) {
			str += connector;
		}
		if (arr[i] !== undefined) {
			str += arr[i];
		}
	}
	return str;
}

var a = new Array( 3 );
fakeJoin( a, "-" ); // "--"
```

如你所见，`join(..)`执行的时候**假定**槽是存在的，然后根据`length`的值来执行循环。而`map(..)`内部的工作原理显然不是这样的，因此那些特殊的“空插槽”数组的结果出乎意料，导致执行失败。

所以，如果你想要**真正**创建一个包含`undefined`值（而不是所谓的“空插槽”）的数组，你会怎么做？

```js
var a = Array.apply( null, { length: 3 } );
a; // [ undefined, undefined, undefined ]
```

懵逼了？没事，简单给你解释下它的工作原理。

`apply(..)`是所有函数都可以使用的工具函数，它通过一种特殊的方式调用函数。

第一个参数是一个`this`对象绑定（在本系列标题为“this和对象原型”中讲过），我们在这不关心它，所以把它设置为`null`。第二个参数要求是个数组（或者像数组的东西——又叫“类数组”对象）。这个“数组”的内容会作为参数传递给题中的函数。

因此`Array.apply(..)`是调用`Array(..)`函数，并把传递进来的值（`{ length: 3 }`对象）作为它的参数。

Inside of `apply(..)`, we can envision there's another `for` loop (kinda like `join(..)` from above) that goes from `0` up to, but not including, `length` (`3` in our case).

在`apply(..)`的内部，我们可以设想有另外一个`for`循环（有点像上面的`join(..)`），从`0`开始循环，但是不包括属性`length`（在我们的例子中是`3`）。

对于每个索引，它会把该索引作为键从对象中取它的值。假设在`apply(..)`函数内部这个数组对象参数被命名为`arr`，属性访问大概是这样的`arr[0]`、`arr[1]`和`arr[2]`。然而，`{ length: 3 }`对象中并没有这些属性，所以这三个属性访问的结果都是`undefined`。

换句话说，调用`Array(..)`的最终效果基本上是这样的`Array(undefined,undefined,undefined)`，这就是我们最终得到一个充满`undefined`值的数组的方式，而不是那些该死的空插槽。

虽然`Array.apply( null, { length: 3 } )`用一种奇怪的方式创建了一个充满`undefined`值的数组，但是它比愚蠢的`Array(3)`空插槽**好得多**，并且**更靠谱**。

底线：**在任何情况下都不要**创建那些奇怪的空插槽数组。千万不要去做。它们是疯子（指不定就上来咬你一口）。

### `Object(..)`, `Function(..)`, and `RegExp(..)`

The `Object(..)`/`Function(..)`/`RegExp(..)` constructors are also generally optional (and thus should usually be avoided unless specifically called for):

```js
var c = new Object();
c.foo = "bar";
c; // { foo: "bar" }

var d = { foo: "bar" };
d; // { foo: "bar" }

var e = new Function( "a", "return a * 2;" );
var f = function(a) { return a * 2; };
function g(a) { return a * 2; }

var h = new RegExp( "^a*b+", "g" );
var i = /^a*b+/g;
```

There's practically no reason to ever use the `new Object()` constructor form, especially since it forces you to add properties one-by-one instead of many at once in the object literal form.

The `Function` constructor is helpful only in the rarest of cases, where you need to dynamically define a function's parameters and/or its function body. **Do not just treat `Function(..)` as an alternate form of `eval(..)`.** You will almost never need to dynamically define a function in this way.

Regular expressions defined in the literal form (`/^a*b+/g`) are strongly preferred, not just for ease of syntax but for performance reasons -- the JS engine precompiles and caches them before code execution. Unlike the other constructor forms we've seen so far, `RegExp(..)` has some reasonable utility: to dynamically define the pattern for a regular expression.

```js
var name = "Kyle";
var namePattern = new RegExp( "\\b(?:" + name + ")+\\b", "ig" );

var matches = someText.match( namePattern );
```

This kind of scenario legitimately occurs in JS programs from time to time, so you'd need to use the `new RegExp("pattern","flags")` form.

### `Date(..)` and `Error(..)`

The `Date(..)` and `Error(..)` native constructors are much more useful than the other natives, because there is no literal form for either.

To create a date object value, you must use `new Date()`. The `Date(..)` constructor accepts optional arguments to specify the date/time to use, but if omitted, the current date/time is assumed.

By far the most common reason you construct a date object is to get the current timestamp value (a signed integer number of milliseconds since Jan 1, 1970). You can do this by calling `getTime()` on a date object instance.

But an even easier way is to just call the static helper function defined as of ES5: `Date.now()`. And to polyfill that for pre-ES5 is pretty easy:

```js
if (!Date.now) {
	Date.now = function(){
		return (new Date()).getTime();
	};
}
```

**Note:** If you call `Date()` without `new`, you'll get back a string representation of the date/time at that moment. The exact form of this representation is not specified in the language spec, though browsers tend to agree on something close to: `"Fri Jul 18 2014 00:31:02 GMT-0500 (CDT)"`.

The `Error(..)` constructor (much like `Array()` above) behaves the same with the `new` keyword present or omitted.

The main reason you'd want to create an error object is that it captures the current execution stack context into the object (in most JS engines, revealed as a read-only `.stack` property once constructed). This stack context includes the function call-stack and the line-number where the error object was created, which makes debugging that error much easier.

You would typically use such an error object with the `throw` operator:

```js
function foo(x) {
	if (!x) {
		throw new Error( "x wasn't provided" );
	}
	// ..
}
```

Error object instances generally have at least a `message` property, and sometimes other properties (which you should treat as read-only), like `type`. However, other than inspecting the above-mentioned `stack` property, it's usually best to just call `toString()` on the error object (either explicitly, or implicitly through coercion -- see Chapter 4) to get a friendly-formatted error message.

**Tip:** Technically, in addition to the general `Error(..)` native, there are several other specific-error-type natives: `EvalError(..)`, `RangeError(..)`, `ReferenceError(..)`, `SyntaxError(..)`, `TypeError(..)`, and `URIError(..)`. But it's very rare to manually use these specific error natives. They are automatically used if your program actually suffers from a real exception (such as referencing an undeclared variable and getting a `ReferenceError` error).

### `Symbol(..)`

New as of ES6, an additional primitive value type has been added, called "Symbol". Symbols are special "unique" (not strictly guaranteed!) values that can be used as properties on objects with little fear of any collision. They're primarily designed for special built-in behaviors of ES6 constructs, but you can also define your own symbols.

Symbols can be used as property names, but you cannot see or access the actual value of a symbol from your program, nor from the developer console. If you evaluate a symbol in the developer console, what's shown looks like `Symbol(Symbol.create)`, for example.

There are several predefined symbols in ES6, accessed as static properties of the `Symbol` function object, like `Symbol.create`, `Symbol.iterator`, etc. To use them, do something like:

```js
obj[Symbol.iterator] = function(){ /*..*/ };
```

To define your own custom symbols, use the `Symbol(..)` native. The `Symbol(..)` native "constructor" is unique in that you're not allowed to use `new` with it, as doing so will throw an error.

```js
var mysym = Symbol( "my own symbol" );
mysym;				// Symbol(my own symbol)
mysym.toString();	// "Symbol(my own symbol)"
typeof mysym; 		// "symbol"

var a = { };
a[mysym] = "foobar";

Object.getOwnPropertySymbols( a );
// [ Symbol(my own symbol) ]
```

While symbols are not actually private (`Object.getOwnPropertySymbols(..)` reflects on the object and reveals the symbols quite publicly), using them for private or special properties is likely their primary use-case. For most developers, they may take the place of property names with `_` underscore prefixes, which are almost always by convention signals to say, "hey, this is a private/special/internal property, so leave it alone!"

**Note:** `Symbol`s are *not* `object`s, they are simple scalar primitives.

### Native Prototypes

Each of the built-in native constructors has its own `.prototype` object -- `Array.prototype`, `String.prototype`, etc.

These objects contain behavior unique to their particular object subtype.

For example, all string objects, and by extension (via boxing) `string` primitives, have access to default behavior as methods defined on the `String.prototype` object.

**Note:** By documentation convention, `String.prototype.XYZ` is shortened to `String#XYZ`, and likewise for all the other `.prototype`s.

* `String#indexOf(..)`: find the position in the string of another substring
* `String#charAt(..)`: access the character at a position in the string
* `String#substr(..)`, `String#substring(..)`, and `String#slice(..)`: extract a portion of the string as a new string
* `String#toUpperCase()` and `String#toLowerCase()`: create a new string that's converted to either uppercase or lowercase
* `String#trim()`: create a new string that's stripped of any trailing or leading whitespace

None of the methods modify the string *in place*. Modifications (like case conversion or trimming) create a new value from the existing value.

By virtue of prototype delegation (see the *this & Object Prototypes* title in this series), any string value can access these methods:

```js
var a = " abc ";

a.indexOf( "c" ); // 3
a.toUpperCase(); // " ABC "
a.trim(); // "abc"
```

The other constructor prototypes contain behaviors appropriate to their types, such as `Number#toFixed(..)` (stringifying a number with a fixed number of decimal digits) and `Array#concat(..)` (merging arrays). All functions have access to `apply(..)`, `call(..)`, and `bind(..)` because `Function.prototype` defines them.

But, some of the native prototypes aren't *just* plain objects:

```js
typeof Function.prototype;			// "function"
Function.prototype();				// it's an empty function!

RegExp.prototype.toString();		// "/(?:)/" -- empty regex
"abc".match( RegExp.prototype );	// [""]
```

A particularly bad idea, you can even modify these native prototypes (not just adding properties as you're probably familiar with):

```js
Array.isArray( Array.prototype );	// true
Array.prototype.push( 1, 2, 3 );	// 3
Array.prototype;					// [1,2,3]

// don't leave it that way, though, or expect weirdness!
// reset the `Array.prototype` to empty
Array.prototype.length = 0;
```

As you can see, `Function.prototype` is a function, `RegExp.prototype` is a regular expression, and `Array.prototype` is an array. Interesting and cool, huh?

#### Prototypes As Defaults

`Function.prototype` being an empty function, `RegExp.prototype` being an "empty" (e.g., non-matching) regex, and `Array.prototype` being an empty array, make them all nice "default" values to assign to variables if those variables wouldn't already have had a value of the proper type.

For example:

```js
function isThisCool(vals,fn,rx) {
	vals = vals || Array.prototype;
	fn = fn || Function.prototype;
	rx = rx || RegExp.prototype;

	return rx.test(
		vals.map( fn ).join( "" )
	);
}

isThisCool();		// true

isThisCool(
	["a","b","c"],
	function(v){ return v.toUpperCase(); },
	/D/
);					// false
```

**Note:** As of ES6, we don't need to use the `vals = vals || ..` default value syntax trick (see Chapter 4) anymore, because default values can be set for parameters via native syntax in the function declaration (see Chapter 5).

One minor side-benefit of this approach is that the `.prototype`s are already created and built-in, thus created *only once*. By contrast, using `[]`, `function(){}`, and `/(?:)/` values themselves for those defaults would (likely, depending on engine implementations) be recreating those values (and probably garbage-collecting them later) for *each call* of `isThisCool(..)`. That could be memory/CPU wasteful.

Also, be very careful not to use `Array.prototype` as a default value **that will subsequently be modified**. In this example, `vals` is used read-only, but if you were to instead make in-place changes to `vals`, you would actually be modifying `Array.prototype` itself, which would lead to the gotchas mentioned earlier!

**Note:** While we're pointing out these native prototypes and some usefulness, be cautious of relying on them and even more wary of modifying them in anyway. See Appendix A "Native Prototypes" for more discussion.

## Review

JavaScript provides object wrappers around primitive values, known as natives (`String`, `Number`, `Boolean`, etc). These object wrappers give the values access to behaviors appropriate for each object subtype (`String#trim()` and `Array#concat(..)`).

If you have a simple scalar primitive value like `"abc"` and you access its `length` property or some `String.prototype` method, JS automatically "boxes" the value (wraps it in its respective object wrapper) so that the property/method accesses can be fulfilled.
