# 从 Python 到 Rust：破解 3 大障碍

> 原文：[`towardsdatascience.com/python-to-rust-breaking-down-3-big-obstacles-094eb99e331d`](https://towardsdatascience.com/python-to-rust-breaking-down-3-big-obstacles-094eb99e331d)

## Python 高手到 Rust 新手——一名数据科学家的过渡故事

[](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)![Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------) [Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------) ·阅读时间 8 分钟·2023 年 12 月 12 日

--

![](img/2c60e597f6041cb1288d3e3032c05b6f.png)

图 1：蛇和螃蟹。（螃蟹：[Romina BM](https://unsplash.com/photos/a-group-of-red-mushrooms-ZWrsjySNfxY)；蛇：[Mohan Moolepetlu](https://unsplash.com/photos/close-up-photo-of-brown-and-gray-snake-VUr5nmC1IM4)；由作者编排）。

我周围的每个人都知道我是一个忠实的🐍 Python 粉丝。我大约 15 年前开始使用 Python，当时我对[Mathworks Matlab](https://www.mathworks.com/products/matlab.html)感到厌倦。尽管 Matlab 的想法看起来不错，但在[掌握 Python](https://medium.com/towards-data-science/master-python-in-10-minutes-a-day-ac32996b5ded)后，我再也没有回头。我甚至在我的大学成为了 Python 的一种布道者，并“传播这个消息”。

> 编写代码的能力并不代表你是一个软件开发人员。

在我目前的雇主[TenneT](https://www.tennet.eu/)——荷兰和德国的大型[传输系统运营商](https://en.wikipedia.org/wiki/Transmission_system_operator)——我们正在与约 10 人的团队一起构建一个*文档解析和验证解决方案*。构建这样的解决方案，尤其是在团队中，比我想象的要困难得多。这也让我对软件工程的正确范式更感兴趣。我一直认为我的代码还不错，但在看到我软件工程师朋友的工作后：天哪，还有很多需要学习的地方！

当我学习[强类型](https://en.wikipedia.org/wiki/Strong_and_weak_typing)、[SOLID 原则](https://en.wikipedia.org/wiki/SOLID)和一般编程架构等主题时，我也略微了解了其他语言以及*它们*如何解决问题。特别是[Rust](https://www.rust-lang.org)吸引了我的注意，因为我经常看到基于 Rust 的 Python 包（例如：[Polars](https://pola.rs/)）。

为了更好地了解 Rust，我跟随了[***官方 Rustlings 课程***](https://github.com/rust-lang/rustlings/)，这是一个包含 96 个小编程*问题*的本地 Git 仓库。虽然它是完全可以做到的，但 Rust 与 Python 非常不同。Rust 编译器非常严格，无法接受*也许*的答案。以下是我认为 Rust 和 Python 之间的三个主要区别。

*免责声明：虽然我对 Python 非常熟练，但我的其他语言有点生疏（双关语）。我仍在学习 Rust，并且可能有些部分理解不完全。*

![](img/18b6b0dc602c2a6cb72766e755250dd8.png)

图 2：我们成功抵达终点（截图由作者提供）。

# 1\. 所有权、借用和生命周期

所有权和借用可能是 Rust 编程语言中最基本的方面。它旨在确保内存安全，无需所谓的垃圾回收器。这是 Rust 的独特概念，我还没有在其他语言中看到过。

让我们从一个例子开始，我们将值`42`赋给变量`answer_of_life`。Rust 现在将在内存中分配一些空间（这稍微复杂一些，但我们现在保持简单），并将*“所有权”*附加到该变量。重要的是要知道一次只能有一个所有者。一些操作“转移所有权”，使得之前的变量引用无效。这通过防止双重释放内存、数据竞争和悬挂引用等问题来确保内存安全。

来源 1：所有权、所有权转移和作用域。

在其他语言中也使用的一个术语是*作用域*。这可以被视为代码*“存在”*的某种区域。每次代码离开一个作用域时，所有拥有所有权的变量都会被解除分配。这是 Python 中根本不同的东西。Python 使用垃圾回收器，在没有对变量的引用时解除分配变量。在*来源 1*的例子中，从变量`s1`到`s2`的所有权转移之后，变量`s1`就不再可用了。

> 作为 Python 用户，所有权可能会让人感到困惑，特别是在开始时确实是一个挑战。

在*来源 1*的例子中有些过于简单。Rust 强制要求你思考变量是如何创建的以及它应该如何被转移。例如，当你将参数传递给函数时，所有权可以被转移，如*来源 2*中所示：

来源 2：一个函数获取所有权，从而使原始变量无效。

仅仅转移所有权可能会很麻烦，甚至对于一些使用场景来说可能无法实现，因此 Rust 提出了一个所谓的*借用*系统。变量通过借用同一个变量来避免转移所有权，而原变量仍然是所有者。默认情况下，借用的变量是不可变的，即只读，但通过添加 `mut` 关键字，借用可以变成可变的。虽然可以有*无限多个不可变借用*，但只允许有一个可变借用。在*源 3*中，我展示了两个不可变借用和一个可变借用的例子。当函数超出作用域时，所有变量将被移除。

源 3：两个不可变和一个可变借用。

生命周期是 Rust 中与借用和所有权相关的概念，帮助编译器强制执行引用有效的时长。你可能会遇到创建一个使用两个借用的结构或函数的情况。这意味着现在函数或结构的结果可能依赖于之前的输入。为了使这一点更明确，我们可以通过注释生命周期来表达关系。在*源 4*中查看示例：

源 4：生命周期语法初看可能令人困惑，但最终会有所帮助。

所有权、借用和生命周期虽然不易处理，但确实迫使你编写更好的代码。至少，当你能够通过编译器的检查时（-:

# 2. Rust 不接受 `None` 作为答案

在 Python 中非常常见的事情在 Rust 中是不可能的：设置一个值为 `None`。这是一个与 Rust 的安全性、可预测性和零成本抽象目标一致的设计选择。

安全性方面类似于 Rust 的所有权、借用和生命周期方面：防止引用指向未分配的内存。通过不允许返回 `None`，将导致更高的可预测性，因为它迫使开发者显式处理可能缺少数字的情况。由于内存安全和可预测行为，Rust 可以实现所有高级语言功能而不会牺牲性能。

> “None shall not pass” — 甘道夫灰袍

仅仅拒绝 `None` 会使 Rust 成为一个糟糕的语言，因此创作者提出了一个不错的替代方案：枚举 `Option` 和 `Result`。通过这些枚举，我们可以显式地表示值的存在或缺失。这也使得错误处理非常优雅。让我们考虑*源 5*中的 `Option` 示例。

源 5：使用 Option 返回可选答案并处理特殊情况。

***等一下！*** 你不是说没有 `None` 吗？这也是第一次让我感到困惑的地方，不过这里的 `None` 是一个特殊的枚举结构体，不接受参数。`Some` 也是一个特殊的结构体，但可以接受参数。我们的函数*divide()*返回这些可能的枚举值之一，随后我们可以检查它是什么，并据此采取行动。

> 没有 `None` 并且强制返回值使得 Rust 非常可预测。

主函数使用 *match* 结构来处理结果，这非常方便。它有点类似于其他语言中的 *switch*/*case* 结构（参见图 2 中 Guido 的回应）。*match* 检查是否是 Enum `Some` 或 Enum `None` 并执行相关操作。

图 3：[Guido van Rossum 对 switch/case 的推文/反应](https://twitter.com/gvanrossum/status/1276336548296810496)。

`Option` 枚举是一种特殊的结构，用于处理可能返回值或不返回值的函数。对于可以返回值或错误的函数，Rust 有一个更为明确的枚举，称为 `Result`。它们的思想完全相同，主要区别在于 `Option` 有一个默认的“错误”值 `None`，而 `Result` 需要一个明确的“错误”类型。这个类型可以是简单的字符串，也可以是更明确的结构体来标识错误。在 *来源 6* 中，divide 函数使用 `Result` 重新编写。

来源 6：`Result` 枚举是一种很好的返回值或错误的方式。

Rust 开发者发现 *match* 结构有时可能有些繁琐，因此添加了 `if let` 和 `while let` 操作符。这些操作符类似于 *match*，但提供了一些漂亮的语法糖和丰富的装饰。甚至还有一个非常酷的 `?` 操作符（这里未展示），它为丰富的装饰添加了一个樱桃在上面！

来源 7：`if let` 和 `while let` 正在创造美丽的语法糖！

使用 Python 时，我学会了使用 Optional 关键字来为结果类型定义值或 None。但我不得不承认 Rust 在这方面处理得非常精妙。我可以想象，Python 社区也会朝着这种风格发展，类似于强类型化的趋势。

# 3\. 类在哪里？

Python 和 Rust 都可以用于两种编程范式：函数式编程（FP）和面向对象编程（OOP）。然而，Rust 实现这些所谓的对象的方式有所不同。在 Python 中，我们有一个典型的 `class` 对象，可以关联变量和方法。像许多其他语言（如 Java）一样，我们现在可以将这个方法作为基础，通过创建继承父类方法和变量的新对象来扩展功能。

在 Rust 中，没有 `class` 关键字，对象与 Python 的根本不同。Rust 使用特质系统来实现代码重用和多态，这可以带来与多重继承相同的好处，但没有多重继承所带来的问题。多重继承通常用于结合或共享多个类的各种功能，但它可能使代码变得复杂和模糊。一个著名的问题是 *钻石* 问题，如 *来源 8* 所示：

来源 8：钻石问题：不清楚使用了哪个方法。

虽然我认为我们可以很容易地解决这个问题，但如果我创建一种新的语言，我也会尝试以不同的方式来做。对于多重继承，目标主要是与其他对象共享相似的功能。在 Rust 中，通过使用 Trait 系统，这种方法做得更优雅。这种方法并不是 Rust 独有的，类似的系统也在 Scala、Kotlin 和 Haskell 中使用。

Rust 中的类是由枚举（Enums）和结构体（Structs）创建的。单独来看，这些只是数据结构，但我们可以向这些类添加功能。我们可以直接这样做，然而，通过使用特征，这些功能可以与多个“类”共享。使用特征的一个大好处是我们可以提前检查某个特征是否被实现。请参见以下示例：

Source 9: 为两个结构体添加一个共享特征。

在这个例子中，我们有一个`Speaker`特征，表示能够说话的角色。我们为两种类型`Jedi`和`Droid`实现了这个特征。每种类型都提供了自己对`speak`方法的实现。

`introduce`函数接受任何实现了`Speaker`特征的类型，并调用`speak`方法。在`main`函数中，我们创建了`Jedi`（Obi-Wan Kenobi）和`Droid`（R2-D2）的实例，并将它们传递给`introduce`函数，展示了多态性。

对我来说，作为一个 Pythonista [🐍](https://emojipedia.org/snake)，Rust 的特征系统非常令人困惑。我花了一段时间才欣赏到其语法的优雅。

# 总结

Rust 是一门非常酷的语言，但绝对不是一门容易学习的语言。Rustlings 课程让我了解了一些基础知识，但我远远没有足够熟练来承担大型项目。但我真的很喜欢 Rust 如何迫使你编写更好、更安全的代码。

Python 仍然是我的日常使用语言。在工作中，我们的文档管道完全用 Python 构建，而且在机器学习领域，我看不到所有的东西都换成另一种语言。Python 实在是太容易学习了，即使你是一个糟糕的开发者（当然不是我 (-;)），你也可以完成工作。

然而，Rust 的势头正在小幅上升。当然，一些包如 Polars 和 Pydantic 是使用 Rust 构建的，但 HuggingFace 也发布了他们自己的第一个用 Rust 构建的机器学习框架版本，名为 Candle。所以我认为学习一点 Rust 并不是一个坏主意！

我下一步（或实际上是当前）Rust 的旅程是使用 Rust 参与[Advent of Code 2023](https://adventofcode.com/)。我还在研究 Leptos，并计划创建一个个人网站。还有很多东西需要学习！

如果你有任何意见，请告诉我！欢迎在[LinkedIn](https://linkedin.com/in/dennisbakhuis)上联系。
