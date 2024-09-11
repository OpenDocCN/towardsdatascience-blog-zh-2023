# 我是如何构建一个编程语言的：通往成功的（艰难）之路

> 原文：[https://towardsdatascience.com/how-i-built-a-programming-language-the-difficult-path-to-success-38aefa2bb69e?source=collection_archive---------14-----------------------#2023-07-20](https://towardsdatascience.com/how-i-built-a-programming-language-the-difficult-path-to-success-38aefa2bb69e?source=collection_archive---------14-----------------------#2023-07-20)

## 坦白说，“艰难”这个词还不足以形容。

[](https://medium.com/@yashrajvishwakarma.31?source=post_page-----38aefa2bb69e--------------------------------)[![Yashrajvishwakarma](../Images/c07b1c3dd52ee792139cb517e8b25661.png)](https://medium.com/@yashrajvishwakarma.31?source=post_page-----38aefa2bb69e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38aefa2bb69e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----38aefa2bb69e--------------------------------) [Yashrajvishwakarma](https://medium.com/@yashrajvishwakarma.31?source=post_page-----38aefa2bb69e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa7fae3e99bf7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-programming-language-the-difficult-path-to-success-38aefa2bb69e&user=Yashrajvishwakarma&userId=a7fae3e99bf7&source=post_page-a7fae3e99bf7----38aefa2bb69e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----38aefa2bb69e--------------------------------) ·6分钟阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38aefa2bb69e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-programming-language-the-difficult-path-to-success-38aefa2bb69e&user=Yashrajvishwakarma&userId=a7fae3e99bf7&source=-----38aefa2bb69e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38aefa2bb69e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-built-a-programming-language-the-difficult-path-to-success-38aefa2bb69e&source=-----38aefa2bb69e---------------------bookmark_footer-----------)

我的[上一篇文章](https://medium.com/@yashrajvishwakarma.31/how-i-built-a-programming-language-acd144f3867f)介绍了我的编程语言的语法，并提供了一个关于我如何构建它的总体思路。

但我决定写另一篇文章，专注于我朝最终结果前进的历程，因为说实话，这段旅程确实有更多的挫折而非成功，我必须克服的挑战也非常令人生畏。希望这篇文章也能成为那些正在进行类似项目、面临相似困难的人们的动力，提醒他们不要放弃，知道曾经有其他人也处于相同的位置，并最终达成了他们的目标。

## 那你为什么应该在意呢？

毕竟，你们都来自不同的背景，无论是数据科学，还是经典的 Python 和 Java。

即使你不感兴趣于构建自己的编程语言，许多我在这次经历中提升的软技能肯定会对你们所有人（尤其是对编程感兴趣的人）有启发作用（希望也是激励作用）。

你写程序时，有多少次每一行看起来都正确，但却得到一个似乎几乎无法调试的神秘错误，这时候你开始失去动力，想着要放弃。

![](../Images/a949b920ff2b7ef1f7b26a0886feab0e.png)

照片由 [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

在构建语言的过程中，我遇到了那些非常常见的问题，经过最终克服和坚持，这些经验希望能够传达一个值得铭记的信息，当你继续阅读这篇文章时！

## 开始动手

这个想法最初是在我上八年级（或者九年级，不太记得了）时产生的，但那时我几乎不了解条件语句和循环，所以我就把它忘了。

但是多年后，同样的想法重新点燃了我的脑海，只是我完全不知道该如何开始。

![](../Images/264518413c257ec524d35235e18b251d.png)

照片由 [Towfiqu barbhuiya](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

就像所有其他我不知道如何做的事情一样，我在谷歌上搜索了一下。

“如何制作自己的编程语言”。

好吧，结果相当有希望。我花了很长时间逐行分析这些代码，抓住了要点：

编程语言有两种类型：解释型和编译型。解释型语言通常构建起来更简单，但较慢。另一方面，编译型语言将你的代码转换为机器代码，然后执行它（编译型语言通常更快）。

构建语言的一般过程是：

+   **定义语言的目的和语法：** 对我来说这很简单（我知道我想构建一个与数学相关的语言），为了节省工作量并使语言尽可能高级，我决定只保留几个（基本且有用的）函数和两个变量，一个函数和一个浮点数。

+   **构建词法分析器和语法分析器：** 词法分析器是任何语言的第一个组件，将代码中的每个单词/短语分类（如关键字、操作符、注释等）。但仅靠这些**词法单元**本身并没有意义，要使其有意义，需要构建一个语法分析器。语法分析器确保词法单元遵循语言的语法，并将其放入抽象语法树（AST）中。

+   **执行代码：** AST 中的每个节点代表我们原始词法分析器收集的一个标记，包括函数名。因此，运行我们的 AST 允许我们逐行执行编写的代码；这就是解释型编程语言的基本原理。

但编译器需要一些额外的步骤；你需要使用流行的库，如 [LLVM](https://llvm.org/docs/tutorial/MyFirstLanguageFrontend/index.html) 和 [libgccjit](https://gcc.gnu.org/onlinedocs/jit/)，将代码转换为编译后的可执行文件。

## 编译型还是解释型？

一方面，我可以创建一种解释型语言，这相当具有挑战性，需要我投入大量时间和精力。另一方面，我可以创建一种编译型语言，这**更具挑战性**，需要我投入更多时间和精力。

但构建自己编译型语言的想法实在是太吸引人了，我陷入了这个陷阱。

***建议：不要这样做。***

尽管有关于使用 LLVM 构建编译器的资源（强烈推荐观看那段 100 秒的视频，既有趣又有些教育意义），但由于文档过于复杂，我很快就迷失了。

我猜测，你可能比学习使用 LLVM 构建编译器的基础知识要更快地学会 Java 的基础知识。

![](../Images/1775d933031de14e3c2886999ed3573a.png)

图片由 [Emile Perron](https://unsplash.com/@emilep?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 巨大的时间浪费（还是说不是）？

对我来说，不幸的是，我花了两个多月的时间，先是折腾 LLVM，然后是 libgccjit。后者看起来更有希望，我甚至为编译器编写了代码，但随后遇到了大量关于设置 libgccjit 包的问题；我甚至下载了它的全部源代码，并将其放在与我的编译器代码相同的目录中。但仍然没有任何效果。我浏览了无数个 Reddit 和 StackOverflow 的帖子，但没有任何结果。

当 StackOverflow 无法帮助你时，你知道情况非常糟糕。

## 处于放弃的边缘

此时，我真的陷入了一个新的低谷，因为我很长时间没有取得丝毫进展，开始贬低自己所做的所有工作，比如从头开始构建词法分析器和解析器，而没有使用流行的 lex/yacc 和 flex/Bison 工具。在这时，我问自己：“我是否应该把这一切都扔进（虚拟的）垃圾桶里？”我很想说“是的”，但我不那么容易放弃，特别是在涉及编程相关的项目时。

所以我去散步，清理了思路，然后回到设计图板，这次的目标不同：构建一个解释型语言。

## 最终完成了它。

一旦我决定要构建一个解释器，我也做出了放弃 C 语言、回到 Python 的艰难决定。在正常情况下，最好使用较低级的语言来构建解释性语言，因为它们的速度弥补了解释器的慢速。

但我当时正处于绝望的时期，那时候需要采取绝望的措施。

回到印度后，我花了接下来的一周处于失眠状态，字面意义上全天24小时工作（如果不算早餐、午餐和晚餐的话，大约是21小时——我吃得很慢）。

接下来发生的事情是我第一次遇到的，并且可能不会再发生——我的代码第一次尝试就成功了！没有调试，没有更多的困惑。

当我运行我的源代码时，每一个组件：从词法分析器到解析器及 AST，都正常工作，并正确执行了每一行代码。

![](../Images/283fa4577fcbe40820ca52b484957066.png)

图片由 [the blowup](https://unsplash.com/@theblowup?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 反思与最终的备注

上面的引用可能听起来有些陈词滥调，但这并不会削弱其有效性；在构建 AdvAnalysis 的过程中，我经历了比成功更多的失败，但最终，我却脱颖而出。

虽然你可以学习在构建自己的编程语言时需要注意的事项（例如编译型与解释型、应遵循的过程），但我得到的主要经验教训（希望你也能得到）是，无论程序中不断遇到令人困惑的错误多么令人沮丧，只要你能够保持耐心，回到起点并采取不同的方法，最终，它总会如你所期待的那样运作。

说实话，作为一名编程爱好者，你可能早就知道了这一点。 :)
