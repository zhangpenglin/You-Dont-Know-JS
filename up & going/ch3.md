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

The third title in this series primarily focuses on tackling yet another highly controversial topic: type coercion. Perhaps no topic causes more frustration with JS developers than when you talk about the confusions surrounding implicit coercion.

By far, the conventional wisdom is that implicit coercion is a "bad part" of the language and should be avoided at all costs. In fact, some have gone so far as to call it a "flaw" in the design of the language. Indeed, there are tools whose entire job is to do nothing but scan your code and complain if you're doing anything even remotely like coercion.

But is coercion really so confusing, so bad, so treacherous, that your code is doomed from the start if you use it?

I say no. After having built up an understanding of how types and values really work in Chapters 1-3, Chapter 4 takes on this debate and fully explains how coercion works, in all its nooks and crevices. We see just what parts of coercion really are surprising and what parts actually make complete sense if given the time to learn.

But I'm not merely suggesting that coercion is sensible and learnable, I'm asserting that coercion is an incredibly useful and totally underestimated tool that *you should be using in your code.* I'm saying that coercion, when used properly, not only works, but makes your code better. All the naysayers and doubters will surely scoff at such a position, but I believe it's one of the main keys to upping your JS game.

Do you want to just keep following what the crowd says, or are you willing to set all the assumptions aside and look at coercion with a fresh perspective? The *Types & Grammar* title of this series will coerce your thinking.

在本系列的第三个冠军主要集中于解决另一个极具争议的话题：强制类型转换。也许没有话题引起与JS开发者更多的挫折比当你谈论周围隐含胁迫的混乱。

到目前为止，传统的看法是，隐含的强制是语言的一个“坏的部分”，并应不惜一切代价避免。事实上，有些人竟然把它在语言设计的一个“缺陷”了。事实上，有工具，其整个工作是做什么，但扫描你的代码，抱怨，如果你甚至远程做什么样的胁迫。

但是，强制真的如此混乱，如此糟糕，那么奸诈，你的代码是从一开始就注定，如果你使用它？

我说没有。在已经建成了怎样类型和值真的第1-3章的工作有所了解，第4章发生在这次辩论，并充分说明了强制如何工作的，在其所有的角落和缝隙。我们看到，正是强迫的部分真的是令人惊讶的，哪些部分实际上使完整意义上的，如果考虑到学习的时间。

但我不只是暗示胁迫是明智的和可以学习的，我断言胁迫是一个非常有用的，完全低估工具，*您应该使用在你的代码。*我是说，胁迫，如果使用得当，不仅有效，而且让你的代码更好。所有反对者和怀疑者一定会嘲笑这样的位置，但我相信它的主键正在增加你的JS游戏之一。

你想只保留以下的人群说什么，或者你愿意抛开所有假设，并期待在胁迫用全新的视角？在*类型和这一系列的语法*标题会强迫你的思维。

## 异步和性能

The first three titles of this series focus on the core mechanics of the language, but the fourth title branches out slightly to cover patterns on top of the language mechanics for managing asynchronous programming. Asynchrony is not only critical to the performance of our applications, it's increasingly becoming *the* critical factor in writability and maintainability.

The book starts first by clearing up a lot of terminology and concept confusion around things like "async," "parallel," and "concurrent," and explains in depth how such things do and do not apply to JS.

Then we move into examining callbacks as the primary method of enabling asynchrony. But it's here that we quickly see that the callback alone is hopelessly insufficient for the modern demands of asynchronous programming. We identify two major deficiencies of callbacks-only coding: *Inversion of Control* (IoC) trust loss and lack of linear reason-ability.

To address these two major deficiencies, ES6 introduces two new mechanisms (and indeed, patterns): promises and generators.

Promises are a time-independent wrapper around a "future value," which lets you reason about and compose them regardless of if the value is ready or not yet. Moreover, they effectively solve the IoC trust issues by routing callbacks through a trustable and composable promise mechanism.

前三标题这一系列集中在语言的核心机制，但在第四标题分支出来微微覆盖上的语言力学用于管理异步编程的顶部图案。异步不仅提供了应用程序的性能关键，它日益成为*在可写性和可维护性*关键因素。

书中首先开始通过清除了很多周围的事物的术语和概念混淆，如“异步”，“平行”和“同步”，并在深入这样的事情怎么做，并不适用于JS解释说。

然后，我们进入检查回调为使不同步的主要方法。但在这里，我们很快看到单独的回调是异步编程的现代需求绝望不足。我们确定了两个重大缺陷回调只编码：*控制*（IOC）的信任丧失和缺乏线性理由能力的反转。

要解决这两个重大缺陷，ES6引入了两个新的机制（而事实上，图案）：承诺和发电机。

承诺是围绕“未来价值”，它可以让你推理，也不管是否值已准备就绪或尚未撰写他们与时间无关包装。此外，他们有效地通过一个可信和可组合的承诺机制路由回调解决了国际奥委会的信任问题。

Generators introduce a new mode of execution for JS functions, whereby the generator can be paused at `yield` points and be resumed asynchronously later. The pause-and-resume capability enables synchronous, sequential looking code in the generator to be processed asynchronously behind the scenes. By doing so, we address the non-linear, non-local-jump confusions of callbacks and thereby make our asynchronous code sync-looking so as to be more reason-able.

But it's the combination of promises and generators that "yields" our most effective asynchronous coding pattern to date in JavaScript. In fact, much of the future sophistication of asynchrony coming in ES7 and later will certainly be built on this foundation. To be serious about programming effectively in an async world, you're going to need to get really comfortable with combining promises and generators.

If promises and generators are about expressing patterns that let our programs run more concurrently and thus get more processing accomplished in a shorter period, JS has many other facets of performance optimization worth exploring.

Chapter 5 delves into topics like program parallelism with Web Workers and data parallelism with SIMD, as well as low-level optimization techniques like ASM.js. Chapter 6 takes a look at performance optimization from the perspective of proper benchmarking techniques, including what kinds of performance to worry about and what to ignore.

Writing JavaScript effectively means writing code that can break the constraint barriers of being run dynamically in a wide range of browsers and other environments. It requires a lot of intricate and detailed planning and effort on our parts to take a program from "it works" to "it works well."

The *Async & Performance* title is designed to give you all the tools and skills you need to write reasonable and performant JavaScript code.

发电机引入执行的JS功能，从而产生可以在`yield`点被暂停和异步后来恢复的新模式。暂停-和恢复能力可在发电机同步，顺序看代码以异步方式在幕后进行处理。通过这样做，我们处理回调的非线性，非本地跳混乱，从而使我们的异步代码同步寻找以便更多的理由，能。

但它的承诺和发电机的“产量”我们最有效的异步编码图案迄今为止在JavaScript中的组合。事实上，大部分的非同步ES7未来的未来复杂的和以后肯定会建立在这个基础。要认真对待异步的世界有效编程，你将需要得到相结合的承诺和发电机真的很舒服。

如果承诺和发电机大约表达模式，让我们的节目更同时运行，从而获得更多的处理在更短的时间完成的，JS具有性能优化值得探讨的许多其他方面。

第5章深入研究了像网络工作者和SIMD数据并行，以及低级别的优化技术，如ASM.js.程序的并行主题第6章看一看在性能优化，从适当的基准测试技术，包括担心什么样的性能问题以及忽略的视角。

编写JavaScript实际上意味着编写代码，可以打破在广泛的浏览器和其他环境的动态中运行的制约障碍。这需要大量的对我们的零件复杂和详细的规划和努力，采取程序从“工作原理”到“它工作得很好。”

在*异步与性能*标题的目的是让你所有你需要编写合理，高性能JavaScript代码的工具和技能。

## ES6及展望

No matter how much you feel you've mastered JavaScript to this point, the truth is that JavaScript is never going to stop evolving, and moreover, the rate of evolution is increasing rapidly. This fact is almost a metaphor for the spirit of this series, to embrace that we'll never fully *know* every part of JS, because as soon as you master it all, there's going to be new stuff coming down the line that you'll need to learn.

This title is dedicated to both the short- and mid-term visions of where the language is headed, not just the *known* stuff like ES6 but the *likely* stuff beyond.

While all the titles of this series embrace the state of JavaScript at the time of this writing, which is mid-way through ES6 adoption, the primary focus in the series has been more on ES5. Now, we want to turn our attention to ES6, ES7, and ...

Since ES6 is nearly complete at the time of this writing, *ES6 & Beyond* starts by dividing up the concrete stuff from the ES6 landscape into several key categories, including new syntax, new data structures (collections), and new processing capabilities and APIs. We cover each of these new ES6 features, in varying levels of detail, including reviewing details that are touched on in other books of this series.

不管你多么觉得自己已经掌握的JavaScript这一点，事实是，JavaScript是永远不会停止进化，而且，进化的速度迅速增加。这一事实几乎是这个系列的精神隐喻，拥抱，我们永远不会完全*知道* JS的每一个部分，因为只要你掌握了这一切，也将是未来的路线新的东西，你“会需要学习。

这个称号是专门为那里的语言为首的短期和中期愿景，不只是* *闻名的东西，如ES6但* *可能超越的东西。

虽然这一系列的所有冠军揽在写这篇文章，这是中途通过ES6通过时的JavaScript的状态，在该系列的主要焦点已经更多的ES5。现在，我们希望把我们的注意ES6，ES7，并...

由于ES6是在写这篇文章的时候几乎完全，* ES6＆Beyond的*开始被瓜分从ES6景观的具体的东西分成几个主要类别，其中包括新的语法，新的数据结构（集合），以及新的处理功能和API 。我们互相覆盖这些新ES6的特性，在不同级别的细节，包括在这一系列的其他书籍触及审查的细节。

Some exciting ES6 things to look forward to reading about: destructuring, default parameter values, symbols, concise methods, computed properties, arrow functions, block scoping, promises, generators, iterators, modules, proxies, weakmaps, and much, much more! Phew, ES6 packs quite a punch!

The first part of the book is a roadmap for all the stuff you need to learn to get ready for the new and improved JavaScript you'll be writing and exploring over the next couple of years.

The latter part of the book turns attention to briefly glance at things that we can likely expect to see in the near future of JavaScript. The most important realization here is that post-ES6, JS is likely going to evolve feature by feature rather than version by version, which means we can expect to see these near-future things coming much sooner than you might imagine.

The future for JavaScript is bright. Isn't it time we start learning it!?

一些令人兴奋的事情ES6期待念叨：解构，默认参数值，符号，简洁的方法，计算性能，箭头功能块作用域，承诺，发电机，迭代器，模块，代理，weakmaps，和很多很多！呼，包ES6相当一记重拳！

本书的第一部分是你需要学习做好准备为你编写和探索，在未来几年新的和改进的JavaScript的所有东西的路线图。

本书的后半部分的东西，我们可以预计可能在JavaScript中的不久的将来看到注意力转向短暂的一瞥。这里最重要的认识是，后ES6，JS很可能将通过功能，而不是通过版本版本进化功能，这意味着我们可以期待看到这些近未来东西进来更快比你想象。

未来针对JavaScript是光明的。是不是时候，我们开始学习吧！？

## 总结

The *YDKJS* series is dedicated to the proposition that all JS developers can and should learn all of the parts of this great language. No person's opinion, no framework's assumptions, and no project's deadline should be the excuse for why you never learn and deeply understand JavaScript.

We take each important area of focus in the language and dedicate a short but very dense book to fully explore all the parts of it that you perhaps thought you knew but probably didn't fully.

"You Don't Know JS" isn't a criticism or an insult. It's a realization that all of us, myself included, must come to terms with. Learning JavaScript isn't an end goal but a process. We don't know JavaScript, yet. But we will!

在* YDKJS*系列是献给所有JS开发人员可以和应该学习这一切伟大的语言部分的命题。任何人的意见，没有任何框架的假设，并没有项目的最后期限应该是为什么你从来没有学习和深刻理解JavaScript的借口。

我们采取集中各重要领域的语言和奉献一个简短但非常密集的书，充分发掘你也许以为你知道的，但可能没有充分的所有部分。

“你不知道JS”是不是批评或侮辱。这是一个认识到我们所有的人，包括我自己，一定要来的条款。学习JavaScript是不是最终目标，而是一个过程。我们不知道的JavaScript，但。但我们会！
