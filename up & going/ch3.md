# 你不知道的JavaScript: Up & Going
# 第三章: 走进YDKJS

这个系列的书在讲什么呢？简单来说，它是关于认真对待如何学习**JavaScript的所有部分**，而不仅仅是这门语言的一些子集，有人称之为“**the good parts**”（译者注：这里指的是Douglas Crockford 写的《JavaScript语言精粹》），也不是让你完成工作任务所需的最少知识。

其他语言中的认真的开发者会尽他们最大的努力去学习大部分或全部的关于他们主要使用的编程语言的相关知识，但是JS开发者似乎独树一帜，因为他们通常不学习很多的关于JavaScript的知识（也能很好的完成他们的工作）。这可不是一件好事，我们不应该让它称为常态。

**你不知道的JavaScript**（**YDKJS**）系列书籍与典型学习JS的方法形成了鲜明的对比，不像几乎其他任何关于JS的书籍。它会不断的挑战你，让你走出你的舒适区，并且在你遇到的每一个行为都会问更深层次的“为什么”的问题。你准备好接受挑战了吗？（原句：The *You Don't Know JS* (*YDKJS*) series stands in stark contrast to the typical approaches to learning JS, and is unlike almost any other JS books you will read. It challenges you to go beyond your comfort zone and to ask the deeper "why" questions for every single behavior you encounter. Are you up for that challenge?）

我打算在最后一章简要地总结一下本系列剩余书籍的内容，以及在**YDKJS**基础之上，如何最有效地建立JS学习的基础。

## 作用域和闭包

在JavaScript中变量作用域是如何工作的，这可能是你需要快速了解的最基本的知识之一。对作用域的了解仅停留在道听途说这是远远不够的（原句：It's not enough to have anecdotal fuzzy *beliefs* about scope）。

在**作用域和闭包**一书中，开篇就揭穿了JS是“解释型语言”这个最常见的误解，当然它也不是编译型的。绝对不是。

JS引擎在执行之前会立即编译你的代码（有些引擎会在执行时编译）。因此，我们从编译器的视角来深入理解我们的代码是如何处理变量和函数声明的。之后，我们会学习JS作用域管理变量，称之为“提升”。（太绕了，原句：The JS engine compiles your code right before (and sometimes during!) execution. So we use some deeper understanding of the compiler's approach to our code to understand how it finds and deals with variable and function declarations. Along the way, we see the typical metaphor for JS variable scope management, "Hoisting."）

对“词法作用域”的深入理解是我们探索最后一章“闭包”的基础。闭包可能是JS中最重要的概念之一，但是如果你一开始没有牢牢掌握作用域是如何工作的，你肯定就无法掌握闭包。

闭包的一个重要应用是模块模式，正如我们在本书第二章简单介绍的那样。模块模式或许是JavaScript中最普遍的代码组织模式；深刻理解它应该是你的首要任务之一。

## this和对象原型

关于JavaScript最普遍和持久的误解（mistruths）可能就是`this`指向它出现在的那个函数。错误至极！

`this`是基于函数执行的上下文来执行动态绑定，这里有四条简单的规则去理解和完全确定`this`绑定到哪。

与`this`密切相关的是对象原型机制，它是一个属性查找链，与词法作用域查找变量类似。将原型包裹起来是JS的另外一大败笔：模拟类和（所谓的“原型”）继承。（译者注：ES6中的类并非静态语言如Java中的类，它是类似于JavaScript原型继承的语法糖，其底层的实现还是通过原型继承来实现的。这段翻译起来感觉很奇怪，为了不误导大家，还请看原句：Closely related to the `this` keyword is the object prototype mechanism, which is a look-up chain for properties, similar to how lexical scope variables are found. But wrapped up in the prototypes is the other huge miscue about JS: the idea of emulating (fake) classes and (so-called "prototypal") inheritance.）

不幸的是，把类和继承的设计模式思想带入JavaScript中是你可以尝试做的最糟糕的事情，因为语法上可能诱导你认为这里确实有类似类的东西存在，其实在它行为背后的基础还是原型机制。

现在问题是，你是忽略这种不匹配假装你在使用“继承”，还是学习和拥抱对象原型系统实际上是如何工作的。后者更恰当的命名为“行为代理”。

这不仅仅是语法偏好。代理是一个完全不同的，功能更加强大机制，而设计模式只是一个用类和继承来弥补语言设计缺陷的东西。当然，我的这些断言肯定会打那些在JavaScript整个生命周期中讨论这个主题的几乎所有的博客、书籍和会议的的脸。（原句：This is more than syntactic preference. Delegation is an entirely different, and more powerful, design pattern, one that replaces the need to design with classes and inheritance. But these assertions will absolutely fly in the face of nearly every other blog post, book, and conference talk on the subject for the entirety of JavaScript's lifetime.）

我做出关于代理与继承的宣言，并非来自对语言和语法的反感，而是发自内心的希望大家看到JS语言真正的能力并发挥它真正的价值，并且希望将那无尽的迷惑和沮丧扫除。（原句：The claims I make regarding delegation versus inheritance come not from a dislike of the language and its syntax, but from the desire to see the true capability of the language properly leveraged and the endless confusion and frustration wiped away.）

我做出关于原型和代理的案例是更贴近JS的特性，以至于我会在这里尽情享受（忽略这蹩脚的翻译，请看原句：But the case I make regarding prototypes and delegation is a much more involved one than what I will indulge here.）。如果你准备重新考虑一切你认为你知道的关于JavaScript的“类”和“继承”，我为你提供了“选择红色药丸”的机会（源自黑客帝国：矩阵，1999），请查阅本系列标题为“this和对象原型”的第四到六章。

## 类型和语法

本系列的第三个主题主要集中在解决另一个极具争议的话题：强制类型转换。也许没有话题比关于隐式转换带来的混乱给JS开发者造成更多的挫败感（原句：Perhaps no topic causes more frustration with JS developers than when you talk about the confusions surrounding implicit coercion）。

到目前为止，传统的看法是隐式转换是语言的一个“糟粕”（“bad part”），应该不惜一切代价避免它。事实上，有人竟然已经认为它是语言的一个设计“缺陷”。事实上，这里有些工具它的整个工作就是扫描你的代码，如果发现你的代码有类似强制转换的东西，就给你报错（原句：Indeed, there are tools whose entire job is to do nothing but scan your code and complain if you're doing anything even remotely like coercion.）。

但是强制转换真的如此混乱、如此糟糕、如此奸诈，如果你在代码中使用它，你的代码从一开始就注定如此吗？

要我说，肯定不是！在阅读完第一到三章，你将会深入理解类型和值是如何的工作的，在第四章我将展开这次辩论，充分说明强制转换在其所有的情况下是如何工作的。如果你肯花时间学习，你将会看到强制转换哪些部分是真的令人惊讶的，哪些部分实际上是有意义的。

但我不只是暗示强制转换是合理且可学习的，我认为强制转换是一个非常有用的、完全被低估的工具，并且我强烈建议你应该在你的代码中使用它。我想说关于强制转换，如果使用得当，不仅有效，而且让你的代码变得更好。所有的反对者和怀疑者肯定会嘲笑这样的观点，但是我坚信它是你升级你JS技能的主要钥匙之一。

你想继续保持那些“大多数”的观点，还是说你愿意抛开所有的臆测，用一个全新的视角去审视“强制转换”？本系列标题为“类型和语法”会强制转换你的思维：）

## 异步和性能

这个系列的前三本书的内容都集中在语言的核心机制，但是第四本书稍微分支出来讲解语言机制的顶层模式：管理异步编程。异步不仅是程序性能的关键，也日益成为在可写性和可维护性上的关键因素。（原句：The first three titles of this series focus on the core mechanics of the language, but the fourth title branches out slightly to cover patterns on top of the language mechanics for managing asynchronous programming. Asynchrony is not only critical to the performance of our applications, it's increasingly becoming *the* critical factor in writability and maintainability.）

书中首先开始通过理清很多关于异步编程的术语和容易混淆的概念，如“异步”、“并行”和“并发”，并且深入解释了这样的事情如何做，哪些不适用于JS。（原句：The book starts first by clearing up a lot of terminology and concept confusion around things like "async," "parallel," and "concurrent," and explains in depth how such things do and do not apply to JS.）

然后，我们讲解使异步成为可能的主要方法回调。但就是在这里，我们看到只有回调是不够的，它很难满足现代异步编程的需求。我们看到只有回调的代码的两个重大缺陷：**控制反转**（IoC，*Inversion of Control*）的不靠谱和缺乏线性编码能力。（原句：Then we move into examining callbacks as the primary method of enabling asynchrony. But it's here that we quickly see that the callback alone is hopelessly insufficient for the modern demands of asynchronous programming. We identify two major deficiencies of callbacks-only coding: *Inversion of Control* (IoC) trust loss and lack of linear reason-ability.）

为了解决这两个重大缺陷，ES6引入了两个新的机制（事实上，是模式）：promises and generators（专业术语，你懂的）。

Promises是一个与时间无关的“未来值”包裹器，它可以让你在不管值是否准备就绪的情况下使用和组合它们。此外，它们将路由回调通过一个可信和可组合的promise（承若）机制，高效地解决了IoC信任问题。（原句：Promises are a time-independent wrapper around a "future value," which lets you reason about and compose them regardless of if the value is ready or not yet. Moreover, they effectively solve the IoC trust issues by routing callbacks through a trustable and composable promise mechanism.）

Generators为JS函数引入一种新的执行模式，它在`yield`点可以被暂停然后可以异步的恢复状态。Generator这种暂定-恢复的能力使得代码看起来是同步顺序的，但幕后却是以异步的方式处理。通过这样做，我们解决了回调带来的非线性和跳跃式的混乱，从而使我们的异步代码看起来像同步调用，这是多么美好的事情。（原句：Generators introduce a new mode of execution for JS functions, whereby the generator can be paused at `yield` points and be resumed asynchronously later. The pause-and-resume capability enables synchronous, sequential looking code in the generator to be processed asynchronously behind the scenes. By doing so, we address the non-linear, non-local-jump confusions of callbacks and thereby make our asynchronous code sync-looking so as to be more reason-able.）

但是，promises和generators的组合才是迄今为止JavaScript中最有效的异步编程模式。事实上，在将要到来的ES7中，大部分关于未来异步的成熟机制，肯定是建立在这个基础上的。如果你想要在异步的世界中更有效的编程，你必须要十分熟悉结合promises和generators。（原句：But it's the combination of promises and generators that "yields" our most effective asynchronous coding pattern to date in JavaScript. In fact, much of the future sophistication of asynchrony coming in ES7 and later will certainly be built on this foundation. To be serious about programming effectively in an async world, you're going to need to get really comfortable with combining promises and generators.）

如果promises和generators是关于如何提高程序的并发性，从而在短时间内处理更多的任务的模式，那JS在关于性能优化的方面有许多值得探索的地方。

第五章深入研究了使用Web Workers来提高程序并行能力和用SIMD提高数据并行，以及低级别的优化技术，如ASM.js。第六章讲解了通过合适的基准测试技术（包括担心什么样的性能问题以及忽略什么问题）的视角来看待性能优化。（原句：Chapter 5 delves into topics like program parallelism with Web Workers and data parallelism with SIMD, as well as low-level optimization techniques like ASM.js. Chapter 6 takes a look at performance optimization from the perspective of proper benchmarking techniques, including what kinds of performance to worry about and what to ignore.）

编写高效的JavaScript意味着编写可以打破在广泛的浏览器和其他环境中动态运行的限制的代码。这需要大量复杂和详细的计划，以及我们自身的努力，将程序从“它能运行”进阶成“它能很好的运行”。（原句：Writing JavaScript effectively means writing code that can break the constraint barriers of being run dynamically in a wide range of browsers and other environments. It requires a lot of intricate and detailed planning and effort on our parts to take a program from "it works" to "it works well."）

“**异步和性能**”这本书提供了所有的工具和技能让你编写合理的、高性能的JavaScript代码。

## 超越ES6

目前为止，不管你觉得自己已经掌握了JavaScript，事实是JavaScript是永远不会停止进化，而且，进化的速度变得更快了。这一事实也是这个系列的精神隐喻，拥抱我们永远不会完全**知道**JS的每一个部分这个事实，因为当你掌握了一切的时候，马上就会有新的东西出现，你又需要继续学习。

这个标题暗指这门语言短期和中期的发展远景，不仅仅是ES6中我们所知道的，也超越了当前的东西。（翻译不来，原句：This title is dedicated to both the short- and mid-term visions of where the language is headed, not just the *known* stuff like ES6 but the *likely* stuff beyond.）

这一系列的书籍都是在拥抱书写本书时候JavaScript当前的状态，而ES6是中途加进来的，所以你会发现本系列的书主要还是集中在ES5上。现在，我希望把我们的注意力集中在ES6，ES7.....

在写这篇文章的时候，ES6基本快完成了，**超越ES6**一书从ES6的视角将具体的东西分成几个主要的类别，其中包括新的语法，新的数据结构（集合），和新的处理功能和API。我们会在不同级别的细节上覆盖这些ES6新特性，包括重新审查我们已经在本系列其他书中提到的细节。

以下是一些ES6值得让我们兴奋的东西：destructuring（解构）, default parameter values（函数默认值）, symbols（符号）, concise methods, computed properties（计算属性）, arrow functions, block scoping（块作用域）, promises, generators, iterators, modules, proxies, weakmaps以及很多很多其他特性！Phew, ES6 packs quite a punch！（大概表达的意思就是ES6真他妈的棒！）

本书第一部分是你所需要学习在未来几年中你将要编写和探索的新的和改进的JavaScript的所有东西的路线图。（真的很绕口啊，原句：The first part of the book is a roadmap for all the stuff you need to learn to get ready for the new and improved JavaScript you'll be writing and exploring over the next couple of years.）

本书后半部分将注意力放在我们希望在不久的将来能在JavaScript中看到的东西。这里最重要的认识是，后ES6时代，JS更可能是根据功能来更新，而不是单纯根据版本来更新，这意味着我们期望在不就的未来可以看到的东西可能比你想象的来得更快。

JavaScript的未来是光明的，是时候开始学习它了！

## 总结

**YDKJS**系列坚持认为所有的JS开发者可以并且应该学习这门伟大语言的所有部分。不用管别人的意见，不用管框架的假设，不用管项目的最后期限，这些都不应该成为你从来不学习和深入理解JavaScript的借口。

我们在语言的各重要领域贡献一本短小精悍的书，充分发掘你认为你已经知道的，但可能没有充分理解的所有部分。

“**You Don't Know JS**”并不是批评或侮辱（在座的各位）。这是我们所有人，包括我自己，一定要意识到的东西。学习JavaScript并不是最终目标，而是一个过程。我们现在不知道的JavaScript，在不远的未来我们肯定会知道！
