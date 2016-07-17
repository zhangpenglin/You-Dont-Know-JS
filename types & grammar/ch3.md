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

`Object(..)`/`Function(..)`/`RegExp(..)`构造函数一般也是可选的（因此通常应该避免使用，除非特别要求）：

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

这里几乎没有理由去使用`new Object()`的构造形式，特别是它强迫你必须一个接一个的添加属性，不像对象字面量形式，一次性可以添加多个属性。

`Function`构造函数只有在极少数情况下才会用到，比如，你需要动态的定义函数参数或函数体。**千万不要把`Function(..)`当作`eval(..)`的另一种形式。**你几乎从不需要以这种方式动态定义函数。

强烈推荐你用字面量形式（`/^a*b+/g`）定义正则表达式，不仅仅是为了语法上便捷，更是基于性能原因——JS引擎会在代码执行前预编译并且缓存它们。不同于我们迄今见过的其他构造形式，`RegExp(..)`具有一定的合理实用性：动态定义正则表达式模式。

```js
var name = "Kyle";
var namePattern = new RegExp( "\\b(?:" + name + ")+\\b", "ig" );

var matches = someText.match( namePattern );
```

在JS程序中经常会出现这种形式的脚本（这是合法的脚本），因此你需要使用`new RegExp("pattern","flags")`这种形式。

### `Date(..)` and `Error(..)`

`Date(..)`和`Error(..)`原生构造函数比其他natives有用得多，因为这里没有任何字面量形式能创建它们。

要创建一个日期对象，你必须使用`new Date()`。`Date(..)`构造器接受可选的参数指定日期或时间，如果省略，就假定是当前时间。

你构造一个日期对象最常见的原因就是为了获取当前的时间戳（自1970年1月1日的毫秒数）。你可以通过调用日期对象实例的`getTime()`来做到这一点。

当更简单的方法是只需调用ES5定义的静态辅助函数：`Date.now()`。对ES5之前的代码进行polyfill也很简单：

```js
if (!Date.now) {
	Date.now = function(){
		return (new Date()).getTime();
	};
}
```

**注意：**如果你调用`Date()`没有带上`new`，你会得到那一刻时间的字符串表示。具体的格式并没有在语言规范里面指定，但浏览器在此基本达成一致：`"Fri Jul 18 2014 00:31:02 GMT-0500 (CDT)"`。

`Error(..)`构造器在有没有`new`关键字表现都是一致的（这与上面的`Array()`很相似）。

你想创建一个错误对象的主要原因是，它捕获了当前执行堆栈的上下文，存入对象（在大多数JS引擎中，一旦创建成功，就暴露一个只读属性`.stack`）。这个堆栈上下文包括了函数调用栈和这个错误对象创建的行数（在代码中哪个位置创建的），这使得调试错误变得更加容易。

你通常会结合`throw`操作符一块使用错误对象：

```js
function foo(x) {
	if (!x) {
		throw new Error( "x wasn't provided" );
	}
	// ..
}
```

错误对象实例一般至少有一个`message`属性，有时会有其他属性（你应该把它们当作只读属性），如`type`。然而，除了检查上述提到的`stack`属性，通常最好的方法是调用错误对象的`toString()`方法（显式调用或通过强制转换隐式调用——参见第四章），以获得一个友好格式的错误信息。

**提示：**技术上讲，除了一般的`Error(..)`native，还有其他一些特定的错误类型natives：`EvalError(..)`、`RangeError(..)`、`ReferenceError(..)`、`SyntaxError(..)`、`TypeError(..)`和`URIError(..)`。但是手动调用这些特定错误的natives非常罕见。当你的程序确实含有真实的异常（如引用未声明的变量会抛出一个`ReferenceError`错误），它们会被自动使用。

### `Symbol(..)`

在ES6中新增了一种原始类型，叫“Symbol”。Symbols是“unique”（不严格保证！）的值，它可以用作对象的属性而不用担心冲突。它们主要是为ES6构造特殊的内置行为，但你也可以定义自己的Symbols。

Symbols可以作为属性名称，但是你不能看到或从程序访问一个symbol的实际值，即使在开发者控制台也不行。如果你在开发者控制台输出一个symbol，你可能会看到类似`Symbol(Symbol.create)`的东西。

在ES6中预定义了几个symbols，你可以通过`Symbol`函数的静态属性进行访问，比如`Symbol.create`、`Symbol.iterator`等，要使用它们，可以这么做：

```js
obj[Symbol.iterator] = function(){ /*..*/ };
```

要自定义symbol，请使用`Symbol(..)`native。`Symbol(..)`native构造器比较特殊，你不允许使用`new`关键字进行构造，因为这么做会引发错误。

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

虽然symbol实际上不是私有（`Object.getOwnPropertySymbols(..)`可以公开的获取对象的symbol）的，但把它们当作私有或特殊属性来使用可能是它们主要的用途。对大多数开发者来说，他们可能会在属性前面加上下划线`_`，这是一个惯例，似乎在告诉别人：“这是私有/专用/内部的属性，请不要动它！”

**注意：**`Symbol`**不是**`object`，它们就是简单的原始类型。

### Native Prototypes

每个内置的native构造器都有它们自己的`.prototype`对象——`Array.prototype`、`String.prototype`等。

These objects contain behavior unique to their particular object subtype.

这些对象包含的行为使得它们的子类型对象变得独特。

例如，所有的字符串对象和通过扩展（即装箱）的字符串原始值，都可以访问这些定义在`String.prototype`对象上的方法。

**注意：**根据文档约定，`String.prototype.XYZ`简写为`String#XYZ`，其他的`.prototype`也是一样的。

* `String#indexOf(..)`：在字符串中找到另一个子字符串的位置
* `String#charAt(..)`：获取字符串指定位置对应的字符
* `String#substr(..)`、`String#substring(..)`和`String#slice(..)`：提取字符串的一部分作为一个新的字符串
* `String#toUpperCase()`和`String#toLowerCase()`：创建一个转换为大写或小写的新字符串
* `String#trim()`：创建一个删除两端空白字符的新字符串

没有任何方法能够直接修改字符串**本身**。修改（如大小写转换或修剪(trimming)）会从现有的值当中创建一个新的值。

通过原型代理（参见本系列标题为“this和对象原型”），任何字符串值都可以访问这些方法：

```js
var a = " abc ";

a.indexOf( "c" ); // 3
a.toUpperCase(); // " ABC "
a.trim(); // "abc"
```

其他的构造器原型包含适合它们类型的行为，比如`Number#toFixed(..)`（将一个数字转为固定位数的数字，并强制转换为字符串返回）和`Array#concat(..)`（合并数组）。所有的函数都可以访问`apply(..)`、`call(..)`和`bind(..)`，因为`Function.prototype`中定义了它们。

但是，一些native的原型不仅仅是纯对象：

```js
typeof Function.prototype;			// "function"
Function.prototype();				// it's an empty function!

RegExp.prototype.toString();		// "/(?:)/" -- empty regex
"abc".match( RegExp.prototype );	// [""]
```

有个特别糟糕的注意，就是你可以修改这些native原型（不只是添加属性）：

```js
Array.isArray( Array.prototype );	// true
Array.prototype.push( 1, 2, 3 );	// 3
Array.prototype;					// [1,2,3]

// don't leave it that way, though, or expect weirdness!
// reset the `Array.prototype` to empty
Array.prototype.length = 0;
```

如你所见，`Function.prototype`是一个函数，`RegExp.prototype`是一个正则表达式，而`Array.prototype`这是一个数组。是不是感觉很有趣，又很酷？

#### Prototypes As Defaults

`Function.prototype`是一个空函数，`RegExp.prototype`是一个“空”（例如，不匹配）的正则表达式，而`Array.prototype`是一个空数组，这让它们很好的给变量赋予**默认值**，如果这些变量当前还没有赋予合适类型的值。

例如：

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

**注意：**在ES6中，我们没必要使用`vals = vals || ..`语法技巧来设置默认值（参见第四章），因为可以在函数声明中通过原生语法来给参数设置默认值（参见第五章）。

这种方法的一个微小好处在于`.prototype`是已经创建和内置的，因此它们只创建**一次**。与之对应，如果使用`[]`、`function(){}`和`/(?:)/`值作为默认值将会在**每次调用**函数`isThisCool(..)`（可能，取决于引擎的实现）重新创建这些值（可能之后会被垃圾回收）。这会浪费内存和CPU的。

此外，要非常小心不要使用`Array.prototype`作为**随后将修改**的默认值。在这个例子中，`vals`用于只读的，但是如果你反过来修改`vals`本身，你实际上是修改`Array.prototype`本身，这将导致前面提到的陷阱！

**注意：**虽然我们指出了这些native原型和它们的一些用途，但依赖它们需谨慎，特别是你无论如何都要修改它们的时候，你需要更加警惕。更多的讨论请参见附录A“Native Prototypes”。

## Review

JavaScript provides object wrappers around primitive values, known as natives (`String`, `Number`, `Boolean`, etc). These object wrappers give the values access to behaviors appropriate for each object subtype (`String#trim()` and `Array#concat(..)`).

If you have a simple scalar primitive value like `"abc"` and you access its `length` property or some `String.prototype` method, JS automatically "boxes" the value (wraps it in its respective object wrapper) so that the property/method accesses can be fulfilled.
