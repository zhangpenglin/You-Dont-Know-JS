# 你不知道的JavaScript: Up & Going
# 第一章: 走进编程

欢迎你进入 **你不知道的JavaScript** (**YDKJS**) 系列.

*Up & Going*介绍了一些基本的编程概念——当然我们会更加倾向于特别介绍JavaScript（经常简称为JS）——以及如何进一步理解本系列的其他主题内容。特别是当你刚接触编程或者JavaScript，这本书会简要的介绍你需要掌握的知识。

这本书一开始就从高级层面解释基本的编程概念。如果你之前没有编程经验并且希望找到一些书来帮助你学习并且透彻的理解JavaScript，这个系列的丛书就是你要找的。

通过第一章你可以找到一些快速入门的东西，来帮助你更多的了解和实践，进入编程的世界。这里有许多很棒的程序设计入门指南帮助你深入探讨这些主题，我建议你向它们学习，并不局限于这一章的介绍。

第1章应该找到一个快速概述的事情你想要更多地了解和实践进入编程。还有其他许多奇妙的编程的介绍资料可以帮助你进一步深入探讨这些话题,我鼓励大家向他们学习本章除了。

第1章应接触作为你要了解和实践进入编程事情的简要概述。也有许多其他的梦幻般的程序设计入门的资源，可以帮助您深入探讨这些主题进一步，我建议你向他们学习，除了这一章。

一旦你了解了编程基础概念，第二章将引导你进入JavaScript的编程世界。第二章介绍了什么是JavaScript，但同样，这不是一个全面的指南——这是**YDKJS**剩余部分要做的工作。

如果你已经熟悉JavaScript了，可以直接阅读第三章，了解一下你可以从*YDKJS*系列中学到什么，然后开始阅读吧！

## 代码

让我们从头开始吧！

程序，通常被称为**源代码**或**代码**，是一组特殊的指令来告诉计算机应该执行什么任务。通常代码保存在文本文件中，尽管你可以直接在浏览器的开发者控制台中输入JavaScript代码，我们接下来会讲解这个。

有效的格式和指令的组合规则被称为**计算机语言**，有时也被叫做（编程语言的）**语法**，就像英语语言告诉你如何拼写单词，以及如何用单词和标点来构造有效的句子。

### 语句

在计算机语言中，执行特定任务的一组单词、数字和运算符称为**语句**。在JavaScript中，语句如下所示：

```js
a = b * 2;
```

字符`a`和`b`称为**变量**（参见**变量**），变量就像一个简单的盒子可以装入你的任何东西。在程序中，变量保存值（如数字`42`），给程序使用。你可以把变量理解为代表值的占位符。

相比之下，`2`仅仅代表值本身，称为**字面量**，因为它是独立的，没有存储在变量中。

字符`=`和`*`称为**运算符**（参见“运算符”）——它们与值和变量执行一些操作，如分配（专业术语称“赋值”）和数学乘法运算。

在JavaScript中大多数语句以分号（;）结尾。

语句`a = b * 2;`告诉计算机，获取变量`b`中的值，乘以`2`，然后将计算结果存入另外一个变量`a`中。

程序就是一组的语句，它们表述了为实现程序目的所需的步骤集合。

### 表达式

语句由一个或者多个**表达式**组成。表达式是一个变量或者值的引用，或者一组变量和值以及运算符的组合。

比如:

```js
a = b * 2;
```

这条语句中有四个表达式：

* `2`是一个**字面量表达式**
* `b`是一个**变量表达式**，表示获取这个变量当前的值
* `b * 2`是一个**算术表达式**，表示做乘法运算
* `a = b * 2`是一个**赋值表达式**，表示将`b * 2`表达式的结果赋予变量`a`（之后会更多的提到赋值操作）

一个单独的一般表达式也被称为**表达式语句**，比如下面：

```js
b * 2;
```

这种表达式语句并不常见或者有用，因为它对程序的运行并没有产生任何影响——它就是将`b`的值取出来，然后乘以`2`，然而并没有对结果进行任何操作。

一种更加常见的表达式语句是**调用表达式**语句（常见“函数”），整个语句就是函数调用表达式本身：

```js
alert( a );
```

### 执行程序

这组编程语句如何告诉计算机该做什么？程序需要被**执行**，也被称为**运行程序**。

像`a = b * 2`这样的语句有利于开发者阅读和书写，但是计算机并不能直接理解这种格式。所以，在电脑上需要一个特殊的工具（可以是**解释器**或者**编译器**）来将你写的代码翻译成计算机能够理解的指令。

对于某些计算机语言，每当程序执行的时候，工具会从上到下，逐行执行这种命令翻译工作，这通常被称为**解释**代码。

对于其他的语言，这种翻译工作会提前完成，称为**编译**代码，所以当之后程序**运行**的时候，计算机实际执行的是编译之后的指令。

很明显，JavaScript是**解释型**语言，因为JavaScript源代码都是在运行时处理的。但这并不完全准确。JavaScript引擎实际上是先**编译**代码，然后立即执行编译后的代码。

**注意：** 有关JavaScript编译的更多信息，请参阅本系列的“作用域和闭包”的前两章。

## 尝试一下

本章将通过一些简单的代码片段来介绍每个编程概念，全部都是用JavaScript写的（你懂的！）。

特别的强调一下：当你阅读本章的时候——你需要花时间多练习几次——你应该通过敲代码来练习每个概念。最简单的方法就是打开你浏览器中的开发者工具（火狐、谷歌浏览器、IE等）。

**提示：**通常情况下，你可以通过键盘快捷键或者菜单项来启动开发者控制台。想要了解更多有关如何在你最喜欢的浏览器中打开和使用控制台的信息，参见“掌握开发者工具控制台”(http://blog.teamtreehouse.com/mastering-developer-tools-console)。要一次输入多行到控制台，使用`<shift> + <enter>`移动到下一行。一旦你输入`<enter>`，控制台就会运行你刚刚输入的代码。

让我们熟悉一下在控制台中运行代码的过程。首先，我建议你在浏览器中打开一个空页面。我更喜欢在地址栏输入`about:blank`来打开空页面。然后，确保你的开发者控制台是打开的，就像我们刚刚提到的那样。

现在，输入这段代码，看看它是如何运行的：

```js
a = 21;

b = a * 2;

console.log( b );
```

前面的代码输入到Chrome的控制台，应该产生类似以下内容：

<img src="fig1.png" width="500">

来吧，试试看吧。学习编程的最好方法是开始编码！

### 输出

在前面的代码片段中，我们使用了`console.log（..）`。让我们简要的讲解下这行代码是什么意思。

你可能已经猜到了，这就是我们在开发者控制台打印文本的方式（又名**输出**给用户）。我们有必要解释一下它的两个特点。

第一，`log( b )`部分被称为函数调用（参见“函数”）。它所做的事情就是，我们将`b`变量交给`log`函数，然后它取出`b`的值把它打印到控制台。

第二，`console.`部分是一个对象引用，函数`log(..)`就在这个对象里面。我们将在第二章中更详细的讲解对象和它的属性。

另一种创建输出的方法就是运行`alert(..)`语句。例如：

```js
alert( b );
```

如果你运行那段代码，你会发现浏览器弹出了一个“OK”对话框，它的内容是变量`b`的值，而不是打印输出到控制台。通常来说，使用`console.log(..)`学习编码并且在控制台运行程序会比使用`alert(..)`更容易，因为你可以一次输出多个值，而且不会打断浏览器界面响应。

在本书中，我们使用`console.log(..)`来输出信息。

### 输入

虽然我们一直在讨论输出，你可能也想知道如何**输入**（即从用户那边接收信息）。

最常见的方式是，在HTML页面中向用户显示表单元素（如文本框），他们可以输入信息到表单中，然后使用JS将用户的输入读取到程序当中。

如果仅仅只是为了简单的学习和演示（就像你当前跟随本书所做的一样），这里有一个更加简单的方法来获取用户的输入，那就是使用`prompt(..)`函数：

```js
age = prompt( "Please tell me your age:" );

console.log( age );
```

你可能已经猜到了，你传递给`prompt(..)`的信息——在这个例子中，`"Please tell me your age:"`——被打印在弹出框里。

这应该类似以下内容：

<img src="fig2.png" width="500">

一旦你点击提交输入的文本，你会发现你输入的值存储在变量`age`中，接着我们使用`console.log(..)`将它**输出**。

<img src="fig3.png" width="500">

为了让我们在学习基本编程概念的时候保持简单，在本书中的例子不需要输入。但是现在你已经了解了如何使用`prompt(..)`，如果你想挑战自己，你可以尝试在接下来的实例是使用输入。

## 运算符

运算符是我们用来操作变量和值的。我们已经看到了两个JavaScript运算符，`=`和`*`。

`*`运算符是用来表示数序乘法的，这很简单吧！

`=`等号运算符用来**赋值**——我们首先计算出`=`**右边**（来源值）的值，然后把它放入`=`**左边**（目标变量）我们指定的变量中。

**警告：**这种以相反方向来赋值的操作可能看起来很奇怪。不像`a = 42`，有些人可能喜欢翻转顺序，使得源值在左边，目标变量在右边，如`42 -> a`（在JavaScript中这是无效的）。不幸的是，像`a = 42`这种有序的形式，以及类似的变形，在现代编程语言中相当普遍。如果你感觉不自然，那就花费一些时间在你心中反复排练下这种顺序，适应它吧。

考虑如下：

```js
a = 2;
b = a + 1;
```

在这里，我们将`2`赋值给变量`a`，然后获取变量`a`的值（仍然是`2`），加`1`得到结果`3`，最后将这个值存储在变量`b`中。

虽然这不是一个技术上的运算符，但是你需要明白每个程序中的关键字`var`，因为它你声明**变量**的主要方式（参见“变量”）。

在使用变量之前，你总是应该通过名字来声明变量。但是，在每个**作用域**（参见“作用域”）你只需要为每个变量声明一次就行，他可以在需要的时候使用任意多次。例如：

```js
var a = 20;

a = a + 1;
a = a * 2;

console.log( a );	// 42
```

以下是JavaScript中一些最常见的运算符：

* 赋值：如：`a = 2`中的`=`
* 算术：`+`（加法），`-`（减法），`*`（乘法）和 `/` (除法)，如：`a * 3`
* 复合赋值：`+=`，`-=`，`*=` 和 `/=` 都是复合赋值运算符，它们组合了算术和赋值两个操作，如：`a += 2` (等同于`a = a + 2`)
* 递加/递减：`++`（递加），`--`（递减），如：`a++`（等同于`a = a + 1`）
* 对象属性访问：如：`console.log()`中的`.` 
对象是在称为属性特定的命名位置持有其他值的值。`obj.a`表示一个对象值，变量名为`obj`，它带有一个叫`a`的属性。也可以通过`obj["a"]`这种方式来访问属性。参见第二章。

* 比较：`==`（一般等于），`===`（严格等于），`!=`（一般不等），`!==`（严格等于），如：`a == b`。
参见第二章的“值与类型”

* 比较：`<`（小与），`>`（大于），`<=`（小于等于），`>=`（大于等于），如：`a <= b`。
参见第二章的“值与类型”

* 逻辑：`&&`（与），`||`（或），如：`a || b`表示选择`a` **或者** `b`
这些操作符是用来表示复合条件语句（参见“条件表达式”），就像`a`**或者**`b`为真。

**注意：**想要了解更多的细节，以及了解这里没有讲解到的运算符，参见Mozilla Developer Network (MDN)'s "Expressions and Operators" (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)。

## 值和类型

如果您在手机商店问员工一部特定的手机的价格，他们说：“99，99”（即，$99.99），他们会告诉你一个实际上的美元数字代表你需要支付（加税）多少钱才能买它。如果你想购买两部，你可以很容易通过心算就知道你需要支付$199.98，一部手机价格的两倍。

如果同样的员工拿起另一个类似的手机，但说这是“免费”的（可能是跟你开玩笑），他们不会给你一个数字，而是另外一种表示价格（￥0.00）的单词——“免费”。

当你接着问手机是否包括充电器，这个问题的答案只能是“是”或“否”。

这很类似于当你在程序中表达值的时候，你会根据它们的用途来选择不同的表示方法。

在编程术语中，这种不同的值表示被称为**类型**。JavaScript内置了一些类型，我们称之为**基本**类型。

* 当你想要做数学运算，你可能想要`number`（数字类型）
* 当你想要往屏幕上打印值，你需要`string`（一个或多个字符，单词，句子）
* 当你需要在程序中做决定，你需要`boolean` (`true` 或 `false`)

被直接包含在源代码中的值称为**字面量**。`string`字面量是由双引号`“...”`或单引号（`'...'`）——唯一的区别是风格偏好。`number`和`boolean`字面量表现为`42`和`true`等。

考虑如下：

```js
"I am a string";
'I am also a string';

42;

true;
false;
```

除了`string`/`number`/`boolean`值类型，很多的编程语言还提供了**数组**、**对象**、**函数**以及更多的类型。我们在本章和接下来的内容，会讲解更多有关值和类型的知识。

### 类型转换

如果你有一个数字类型的值，但是需要把它打印到屏幕上，这个时候你需要将这个数字转换为字符串，在JavaScript中，这个转换称之为**强制转换**。同样，如果你进入了包含一系列数字的电子商务网站上，那些都是字符串，但是如果你需要用这些字符串数字进行算术运算，你就需要把字符串**强制**转换成数字。

JavaScript提供了几种不同的方法用来在**类型**之间进行强制转换。例如：

```js
var a = "42";
var b = Number( a );

console.log( a );	// "42"
console.log( b );	// 42
```

使用如上所示的`Number(..)`（一个内置函数）是一种**显式**强制转换，它能把任何类型转换为`number`（数字）类型。这应该最直截了当的。

但是一个有争议的话题是，当你尝试比较两个不同类型的值的时候，你需要**隐式**强制转换。

当比较字符串`“99.99”`和数字`99.99`，大多数人都认为它们是等价的。但他们不完全一样的，不是吗？它们是有着相同值的不同表示，两种不同的**类型**。你可以说它们是“非严格相等”，难道不是吗？

为了帮助你应对这些常见的情况，JavaScript有时会进行**隐式**强制转换值使得他们的类型相匹配。

所以，如果你使用非严格等于`==`运算符对`"99.99" == 99.99`做比较，JavaScript会将左边的`"99.99"`转换为与它等价的`number`（数字）值`99.99`。然后比较就变成了`99.99 == 99.99`，这个结果当然就是`true`。

虽然旨在帮助你，但是如果你没有花时间去学习规则控制它的行为，隐式转换会给你造成混乱。许多的JS开发者从来没有花时间了解过，所以大家普遍的感觉是，隐式转换是混乱的，会导致程序出现意外的bug，因此，应该尽量避免它。它甚至有时被认为是语言的设计缺陷。

然而，隐式强制是**可以学会**的技巧，而且**应该被认真的学习**，如果你想要认真学习JavaScript编程。一旦你掌握了这些规则，它不仅不混淆，还可以让你的程序变得更好！这种努力是值得的。

**注意：**有关强制的更多信息，请参阅本题第二章和本系列中“类型和语法”的第四章。

## 代码注释

手机店员工可能会记下新发布手机的功能或者公司的新计划。这些笔记只是为了员工自己——而不是给他们的客户阅读的。然而，通过记录他们应该告诉客户什么以及如何告诉客户，能够更好的帮助员工做好他们的工作。

学习写代码最重要的一课就是你必须明白，你写代码不仅仅是为计算机而写代码。对编译器来说代码仅仅只是比特而已，但对开发者来说却不是。（感觉翻译不来，给出原文：One of the most important lessons you can learn about writing code is that it's not just for the computer. Code is every bit as much, if not more, for the developer as it is for the compiler.）

你的计算机只关心机器码（编译器编译之后的一串0和1的序列）。在你的职业生涯中，你可能会写很多的代码，但是最终都会被转换成机器码。你所做的如何编写你的代码的决定，将不仅仅对你、对你的其他团队成员甚至未来的你自己产生极大的影响。

你应该努力写出不仅仅是可正常运行的代码，还需要在检查的时候也有意义。尽量给变量（参见“变量”）和函数（参见“函数”）选择一个好的名称，能够让你的程序生涯走得更远。

另外一个重要的部分就是代码注释。它们是插入你代码中的一些文本，用来向人解释你的代码。解释器或者编译器会忽略掉这些注释。

这里有许多的建议，关于如何写出良好注释的代码；我们无法定义绝对通用的规则。但是一些建议和指导还是十分有用的：

* 没有注释的代码是不合格的代码
* 过度的注释（例如：每行都有）可能是写得不好的代码的标志
* 注释应该解释**为什么**，而不是**这是什么**。如果代码容易引起混乱，注释也可以解释**如何实现**。

在JavaScript中，有两种类型的注释：单行注释和多行注释。

考虑如下:

```js
// This is a single-line comment

/* But this is
       a multiline
             comment.
                      */
```

如果你打算把评论正上方一条语句，或者是在一行的末尾注释，`//`单行注释是合适的。在`//`后面的一切被视为注释（并因此被编译器忽略），一直到行的结尾。单行注释中可以出现任何字符。

考虑如下:

```js
var a = 42;		// 42 is the meaning of life
```

如果你需要在多行注释你的代码，那`/* .. */`多行注释是合适的。

下面是多行注释的一个常见用法：

```js
/* The following value is used because
   it has been shown that it answers
   every question in the universe. */
var a = 42;
```

它可以出现在一行的任意位置，甚至在一行的中间，因为`*/`表示注释结束。例如：

```js
var a = /* arbitrary value */ 42;

console.log( a );	// 42
```

在多行注释的内容中不能出现`*/`，因为它会被解释器认为是结束注释。

你一定会希望通过写注释代码的习惯出发，开始编程的学习。在本章的其余部分，你会看到我用注释来解释的东西，所以在自己的实践中做同样的事情。相信我，大家谁读你的代码都会感谢你的！

## Variables

Most useful programs need to track a value as it changes over the course of the program, undergoing different operations as called for by your program's intended tasks.

The easiest way to go about that in your program is to assign a value to a symbolic container, called a *variable* -- so called because the value in this container can *vary* over time as needed.

In some programming languages, you declare a variable (container) to hold a specific type of value, such as `number` or `string`. *Static typing*, otherwise known as *type enforcement*, is typically cited as a benefit for program correctness by preventing unintended value conversions.

Other languages emphasize types for values instead of variables. *Weak typing*, otherwise known as *dynamic typing*, allows a variable to hold any type of value at any time. It's typically cited as a benefit for program flexibility by allowing a single variable to represent a value no matter what type form that value may take at any given moment in the program's logic flow.

JavaScript uses the latter approach, *dynamic typing*, meaning variables can hold values of any *type* without any *type* enforcement.

As mentioned earlier, we declare a variable using the `var` statement -- notice there's no other *type* information in the declaration. Consider this simple program:

```js
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```

The `amount` variable starts out holding the number `99.99`, and then holds the `number` result of `amount * 2`, which is `199.98`.

The first `console.log(..)` command has to *implicitly* coerce that `number` value to a `string` to print it out.

Then the statement `amount = "$" + String(amount)` *explicitly* coerces the `199.98` value to a `string` and adds a `"$"` character to the beginning. At this point, `amount` now holds the `string` value `"$199.98"`, so the second `console.log(..)` statement doesn't need to do any coercion to print it out.

JavaScript developers will note the flexibility of using the `amount` variable for each of the `99.99`, `199.98`, and the `"$199.98"` values. Static-typing enthusiasts would prefer a separate variable like `amountStr` to hold the final `"$199.98"` representation of the value, because it's a different type.

Either way, you'll note that `amount` holds a running value that changes over the course of the program, illustrating the primary purpose of variables: managing program *state*.

In other words, *state* is tracking the changes to values as your program runs.

Another common usage of variables is for centralizing value setting. This is more typically called *constants*, when you declare a variable with a value and intend for that value to *not change* throughout the program.

You declare these *constants*, often at the top of a program, so that it's convenient for you to have one place to go to alter a value if you need to. By convention, JavaScript variables as constants are usually capitalized, with underscores `_` between multiple words.

Here's a silly example:

```js
var TAX_RATE = 0.08;	// 8% sales tax

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log( amount );				// 215.9784
console.log( amount.toFixed( 2 ) );	// "215.98"
```

**Note:** Similar to how `console.log(..)` is a function `log(..)` accessed as an object property on the `console` value, `toFixed(..)` here is a function that can be accessed on `number` values. JavaScript `number`s aren't automatically formatted for dollars -- the engine doesn't know what your intent is and there's no type for currency. `toFixed(..)` lets us specify how many decimal places we'd like the `number` rounded to, and it produces the `string` as necessary.

The `TAX_RATE` variable is only *constant* by convention -- there's nothing special in this program that prevents it from being changed. But if the city raises the sales tax rate to 9%, we can still easily update our program by setting the `TAX_RATE` assigned value to `0.09` in one place, instead of finding many occurrences of the value `0.08` strewn throughout the program and updating all of them.

The newest version of JavaScript at the time of this writing (commonly called "ES6") includes a new way to declare *constants*, by using `const` instead of `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ..
```

Constants are useful just like variables with unchanged values, except that constants also prevent accidentally changing value somewhere else after the initial setting. If you tried to assign any different value to `TAX_RATE` after that first declaration, your program would reject the change (and in strict mode, fail with an error -- see "Strict Mode" in Chapter 2).

By the way, that kind of "protection" against mistakes is similar to the static-typing type enforcement, so you can see why static types in other languages can be attractive!

**Note:** For more information about how different values in variables can be used in your programs, see the *Types & Grammar* title of this series.

## Blocks

The phone store employee must go through a series of steps to complete the checkout as you buy your new phone.

Similarly, in code we often need to group a series of statements together, which we often call a *block*. In JavaScript, a block is defined by wrapping one or more statements inside a curly-brace pair `{ .. }`. Consider:

```js
var amount = 99.99;

// a general block
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

This kind of standalone `{ .. }` general block is valid, but isn't as commonly seen in JS programs. Typically, blocks are attached to some other control statement, such as an `if` statement (see "Conditionals") or a loop (see "Loops"). For example:

```js
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

We'll explain `if` statements in the next section, but as you can see, the `{ .. }` block with its two statements is attached to `if (amount > 10)`; the statements inside the block will only be processed if the conditional passes.

**Note:** Unlike most other statements like `console.log(amount);`, a block statement does not need a semicolon (`;`) to conclude it.

## Conditionals

"Do you want to add on the extra screen protectors to your purchase, for $9.99?" The helpful phone store employee has asked you to make a decision. And you may need to first consult the current *state* of your wallet or bank account to answer that question. But obviously, this is just a simple "yes or no" question.

There are quite a few ways we can express *conditionals* (aka decisions) in our programs.

The most common one is the `if` statement. Essentially, you're saying, "*If* this condition is true, do the following...". For example:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```

The `if` statement requires an expression in between the parentheses `( )` that can be treated as either `true` or `false`. In this program, we provided the expression `amount < bank_balance`, which indeed will either evaluate to `true` or `false` depending on the amount in the `bank_balance` variable.

You can even provide an alternative if the condition isn't true, called an `else` clause. Consider:

```js
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// can we afford the extra purchase?
if ( amount < bank_balance ) {
	console.log( "I'll take the accessory!" );
	amount = amount + ACCESSORY_PRICE;
}
// otherwise:
else {
	console.log( "No, thanks." );
}
```

Here, if `amount < bank_balance` is `true`, we'll print out `"I'll take the accessory!"` and add the `9.99` to our `amount` variable. Otherwise, the `else` clause says we'll just politely respond with `"No, thanks."` and leave `amount` unchanged.

As we discussed in "Values & Types" earlier, values that aren't already of an expected type are often coerced to that type. The `if` statement expects a `boolean`, but if you pass it something that's not already `boolean`, coercion will occur.

JavaScript defines a list of specific values that are considered "falsy" because when coerced to a `boolean`, they become `false` -- these include values like `0` and `""`. Any other value not on the "falsy" list is automatically "truthy" -- when coerced to a `boolean` they become `true`. Truthy values include things like `99.99` and `"free"`. See "Truthy & Falsy" in Chapter 2 for more information.

*Conditionals* exist in other forms besides the `if`. For example, the `switch` statement can be used as a shorthand for a series of `if..else` statements (see Chapter 2). Loops (see "Loops") use a *conditional* to determine if the loop should keep going or stop.

**Note:** For deeper information about the coercions that can occur implicitly in the test expressions of *conditionals*, see Chapter 4 of the *Types & Grammar* title of this series.

## Loops

During busy times, there's a waiting list for customers who need to speak to the phone store employee. While there's still people on that list, she just needs to keep serving the next customer.

Repeating a set of actions until a certain condition fails -- in other words, repeating only while the condition holds -- is the job of programming loops; loops can take different forms, but they all satisfy this basic behavior.

A loop includes the test condition as well as a block (typically as `{ .. }`). Each time the loop block executes, that's called an *iteration*.

For example, the `while` loop and the `do..while` loop forms illustrate the concept of repeating a block of statements until a condition no longer evaluates to `true`:

```js
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

The only practical difference between these loops is whether the conditional is tested before the first iteration (`while`) or after the first iteration (`do..while`).

In either form, if the conditional tests as `false`, the next iteration will not run. That means if the condition is initially `false`, a `while` loop will never run, but a `do..while` loop will run just the first time.

Sometimes you are looping for the intended purpose of counting a certain set of numbers, like from `0` to `9` (ten numbers). You can do that by setting a loop iteration variable like `i` at value `0` and incrementing it by `1` each iteration.

**Warning:** For a variety of historical reasons, programming languages almost always count things in a zero-based fashion, meaning starting with `0` instead of `1`. If you're not familiar with that mode of thinking, it can be quite confusing at first. Take some time to practice counting starting with `0` to become more comfortable with it!

The conditional is tested on each iteration, much as if there is an implied `if` statement inside the loop.

We can use JavaScript's `break` statement to stop a loop. Also, we can observe that it's awfully easy to create a loop that would otherwise run forever without a `break`ing mechanism.

Let's illustrate:

```js
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

**Warning:** This is not necessarily a practical form you'd want to use for your loops. It's presented here for illustration purposes only.

While a `while` (or `do..while`) can accomplish the task manually, there's another syntactic form called a `for` loop for just that purpose:

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```

As you can see, in both cases the conditional `i <= 9` is `true` for the first 10 iterations (`i` of values `0` through `9`) of either loop form, but becomes `false` once `i` is value `10`.

The `for` loop has three clauses: the initialization clause (`var i=0`), the conditional test clause (`i <= 9`), and the update clause (`i = i + 1`). So if you're going to do counting with your loop iterations, `for` is a more compact and often easier form to understand and write.

There are other specialized loop forms that are intended to iterate over specific values, such as the properties of an object (see Chapter 2) where the implied conditional test is just whether all the properties have been processed. The "loop until a condition fails" concept holds no matter what the form of the loop.

## Functions

The phone store employee probably doesn't carry around a calculator to figure out the taxes and final purchase amount. That's a task she needs to define once and reuse over and over again. Odds are, the company has a checkout register (computer, tablet, etc.) with those "functions" built in.

Similarly, your program will almost certainly want to break up the code's tasks into reusable pieces, instead of repeatedly repeating yourself repetitiously (pun intended!). The way to do this is to define a `function`.

A function is generally a named section of code that can be "called" by name, and the code inside it will be run each time. Consider:

```js
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```

Functions can optionally take arguments (aka parameters) -- values you pass in. And they can also optionally return a value back.

```js
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
```

The function `printAmount(..)` takes a parameter that we call `amt`. The function `formatAmount()` returns a value. Of course, you can also combine those two techniques in the same function.

Functions are often used for code that you plan to call multiple times, but they can also be useful just to organize related bits of code into named collections, even if you only plan to call them once.

Consider:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

Although `calculateFinalPurchaseAmount(..)` is only called once, organizing its behavior into a separate named function makes the code that uses its logic (the `amount = calculateFinal...` statement) cleaner. If the function had more statements in it, the benefits would be even more pronounced.

### Scope

If you ask the phone store employee for a phone model that her store doesn't carry, she will not be able to sell you the phone you want. She only has access to the phones in her store's inventory. You'll have to try another store to see if you can find the phone you're looking for.

Programming has a term for this concept: *scope* (technically called *lexical scope*). In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function's *scoped* variables.

A variable name has to be unique within the same scope -- there can't be two different `a` variables sitting right next to each other. But the same variable name `a` could appear in different scopes.

```js
function one() {
	// this `a` only belongs to the `one()` function
	var a = 1;
	console.log( a );
}

function two() {
	// this `a` only belongs to the `two()` function
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```

Also, a scope can be nested inside another scope, just like if a clown at a birthday party blows up one balloon inside another balloon. If one scope is nested inside another, code inside the innermost scope can access variables from either scope.

Consider:

```js
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// we can access both `a` and `b` here
		console.log( a + b );	// 3
	}

	inner();

	// we can only access `a` here
	console.log( a );			// 1
}

outer();
```

Lexical scope rules say that code in one scope can access variables of either that scope or any scope outside of it.

So, code inside the `inner()` function has access to both variables `a` and `b`, but code in `outer()` has access only to `a` -- it cannot access `b` because that variable is only inside `inner()`.

Recall this code snippet from earlier:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}
```

The `TAX_RATE` constant (variable) is accessible from inside the `calculateFinalPurchaseAmount(..)` function, even though we didn't pass it in, because of lexical scope.

**Note:** For more information about lexical scope, see the first three chapters of the *Scope & Closures* title of this series.

## Practice

There is absolutely no substitute for practice in learning programming. No amount of articulate writing on my part is alone going to make you a programmer.

With that in mind, let's try practicing some of the concepts we learned here in this chapter. I'll give the "requirements," and you try it first. Then consult the code listing below to see how I approached it.

* Write a program to calculate the total price of your phone purchase. You will keep purchasing phones (hint: loop!) until you run out of money in your bank account. You'll also buy accessories for each phone as long as your purchase amount is below your mental spending threshold.
* After you've calculated your purchase amount, add in the tax, then print out the calculated purchase amount, properly formatted.
* Finally, check the amount against your bank account balance to see if you can afford it or not.
* You should set up some constants for the "tax rate," "phone price," "accessory price," and "spending threshold," as well as a variable for your "bank account balance.""
* You should define functions for calculating the tax and for formatting the price with a "$" and rounding to two decimal places.
* **Bonus Challenge:** Try to incorporate input into this program, perhaps with the `prompt(..)` covered in "Input" earlier. You may prompt the user for their bank account balance, for example. Have fun and be creative!

OK, go ahead. Try it. Don't peek at my code listing until you've given it a shot yourself!

**Note:** Because this is a JavaScript book, I'm obviously going to solve the practice exercise in JavaScript. But you can do it in another language for now if you feel more comfortable.

Here's my JavaScript solution for this exercise:

```js
const SPENDING_THRESHOLD = 200;
const TAX_RATE = 0.08;
const PHONE_PRICE = 99.99;
const ACCESSORY_PRICE = 9.99;

var bank_balance = 303.91;
var amount = 0;

function calculateTax(amount) {
	return amount * TAX_RATE;
}

function formatAmount(amount) {
	return "$" + amount.toFixed( 2 );
}

// keep buying phones while you still have money
while (amount < bank_balance) {
	// buy a new phone!
	amount = amount + PHONE_PRICE;

	// can we afford the accessory?
	if (amount < SPENDING_THRESHOLD) {
		amount = amount + ACCESSORY_PRICE;
	}
}

// don't forget to pay the government, too
amount = amount + calculateTax( amount );

console.log(
	"Your purchase: " + formatAmount( amount )
);
// Your purchase: $334.76

// can you actually afford this purchase?
if (amount > bank_balance) {
	console.log(
		"You can't afford this purchase. :("
	);
}
// You can't afford this purchase. :(
```

**Note:** The simplest way to run this JavaScript program is to type it into the developer console of your nearest browser.

How did you do? It wouldn't hurt to try it again now that you've seen my code. And play around with changing some of the constants to see how the program runs with different values.

## 小结

Learning programming doesn't have to be a complex and overwhelming process. There are just a few basic concepts you need to wrap your head around.

These act like building blocks. To build a tall tower, you start first by putting block on top of block on top of block. The same goes with programming. Here are some of the essential programming building blocks:

* You need *operators* to perform actions on values.
* You need values and *types* to perform different kinds of actions like math on `number`s or output with `string`s.
* You need *variables* to store data (aka *state*) during your program's execution.
* You need *conditionals* like `if` statements to make decisions.
* You need *loops* to repeat tasks until a condition stops being true.
* You need *functions* to organize your code into logical and reusable chunks.

Code comments are one effective way to write more readable code, which makes your program easier to understand, maintain, and fix later if there are problems.

Finally, don't neglect the power of practice. The best way to learn how to write code is to write code.

I'm excited you're well on your way to learning how to code, now! Keep it up. Don't forget to check out other beginner programming resources (books, blogs, online training, etc.). This chapter and this book are a great start, but they're just a brief introduction.

The next chapter will review many of the concepts from this chapter, but from a more JavaScript-specific perspective, which will highlight most of the major topics that are addressed in deeper detail throughout the rest of the series.