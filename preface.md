# 你不知道的JavaScript
# 前言

I'm sure you noticed, but "JS" in the book series title is not an abbreviation for words used to curse about JavaScript, though cursing at the language's quirks is something we can probably all identify with!

在互联网发展的早期，JavaScript就已经成为了支撑网页内容交互体验的基础技术。那时JavaScript的作用可能仅仅是生成一些闪烁的鼠标轨迹或者烦人的弹出窗口，但是经过了大约20年的发展，JavaScript的技术和能力都发生了天翻地覆的变化，现在的JavaScript毫无疑问已经成为了世界上使用范围最广的软件平台————互联网————的核心技术。

但是作为一个语言来说，它总是成为大家批评的对象，部分原因是它有很多历史遗留问题，但主要原因是它的设计哲学有问题。就像Brendan Eich曾经说过的，JavaScript甚至连名字都给人一种“蠢弟弟”的感觉，就像是它更成熟的大哥Java的不完整版本。不过名字只不过是营销策略上的一个意外，这两个语言有许多本质上的区别。JavaScript和Java的关系，就像Carnival（嘉年华）和Car（汽车）的关系一样，八杆子打不着。

Because JavaScript borrows concepts and syntax idioms from several languages, including proud C-style procedural roots as well as subtle, less obvious Scheme/Lisp-style functional roots, it is exceedingly approachable to a broad audience of developers, even those with just little to no programming experience. The "Hello World" of JavaScript is so simple that the language is inviting and easy to get comfortable with in early exposure.
JavaScript借鉴了许多语言的概念和语法，比如C风格的过程式

While JavaScript is perhaps one of the easiest languages to get up and running with, its eccentricities make solid mastery of the language a vastly less common occurrence than in many other languages. Where it takes a pretty in-depth knowledge of a language like C or C++ to write a full-scale program, full-scale production JavaScript can, and often does, barely scratch the surface of what the language can do.

Sophisticated concepts which are deeply rooted into the language tend instead to surface themselves in *seemingly* simplistic ways, such as passing around functions as callbacks, which encourages the JavaScript developer to just use the language as-is and not worry too much about what's going on under the hood.

It is simultaneously a simple, easy-to-use language that has broad appeal, and a complex and nuanced collection of language mechanics which without careful study will elude *true understanding* even for the most seasoned of JavaScript developers.

Therein lies the paradox of JavaScript, the Achilles' Heel of the language, the challenge we are presently addressing. Because JavaScript *can* be used without understanding, the understanding of the language is often never attained.

## 使命

If at every point that you encounter a surprise or frustration in JavaScript, your response is to add it to the blacklist, as some are accustomed to doing, you soon will be relegated to a hollow shell of the richness of JavaScript.

While this subset has been famously dubbed "The Good Parts", I would implore you, dear reader, to instead consider it the "The Easy Parts", "The Safe Parts", or even "The Incomplete Parts".

This *You Don't Know JavaScript* book series offers a contrary challenge: learn and deeply understand *all* of JavaScript, even and especially "The Tough Parts".

Here, we address head on the tendency of JS developers to learn "just enough" to get by, without ever forcing themselves to learn exactly how and why the language behaves the way it does. Furthermore, we eschew the common advice to *retreat* when the road gets rough.

I am not content, nor should you be, at stopping once something *just works*, and not really knowing *why*. I gently challenge you to journey down that bumpy "road less traveled" and embrace all that JavaScript is and can do. With that knowledge, no technique, no framework, no popular buzzword acronym of the week, will be beyond your understanding.

These books each take on specific core parts of the language which are most commonly misunderstood or under-understood, and dive very deep and exhaustively into them. You should come away from reading with a firm confidence in your understanding, not just of the theoretical, but the practical "what you need to know" bits.

The JavaScript you know *right now* is probably *parts* handed down to you by others who've been burned by incomplete understanding. *That* JavaScript is but a shadow of the true language. You don't *really* know JavaScript, *yet*, but if you dig into this series, you *will*. Read on, my friends. JavaScript awaits you.

## 总结

JavaScript非常特殊，只学一部分的话非常简单，但是想要完整地学习会很难（就算学到够用也不容易）。当开发者感到迷惑时，他们通常会责怪语言本身，而不是怪自己对语言缺乏了解。这个系列就是为了解决这个问题，让你打心眼儿里欣赏这门语言。

注意: 本书中的许多例子都需要运行在即将到来的现代JavaScript引擎环境中，比如：ES6。部分代码在旧（ES6之前的）引擎上可能无法正常运行。
