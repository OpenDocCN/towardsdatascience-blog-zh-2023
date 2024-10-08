# 懒惰评估使用递归 Python 生成器

> 原文：[`towardsdatascience.com/lazy-evaluation-using-recursive-python-generators-9ee6af0dd803`](https://towardsdatascience.com/lazy-evaluation-using-recursive-python-generators-9ee6af0dd803)

## 递归函数可以使用*“懒惰评估”*吗？——是的，它们可以——通过使用 Python 的生成器函数！

[](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)![Martin Heinz](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------) [Martin Heinz](https://medium.com/@martin.heinz?source=post_page-----9ee6af0dd803--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9ee6af0dd803--------------------------------) ·5 分钟阅读·2023 年 1 月 4 日

--

![](img/84db954d46d54cd7ea660368b0737fa3.png)

生成自稳定扩散

我们都熟悉 Python 的生成器及其所有优点。但是，如果我告诉你我们可以通过将生成器与递归结合起来，使它们变得更好呢？所以，让我们看看如何利用它们来实现*“懒惰递归”*，并提升我们在 Python 中使用生成器的效果！

# 为什么还要费心？

在我们进入代码之前，先问自己*“为什么还要费心？我们真的需要递归生成器吗？”*答案是……这要视情况而定。自然，递归生成器将会兼具生成器和普通递归函数的优缺点。

对于生成器而言，使用它们的首要原因是*“懒惰”*评估——即一次计算一个元素，而不是一次计算全部。至于递归，它对某些算法或问题的解决方式是自然的，例如树的遍历。

因此，递归生成器适合的情况自然是递归算法，这些算法可能处理大量数据或元素，因此如果*“急切”*地运行，会消耗大量内存。

# 基本示例

现在我们知道了*为什么*我们会使用递归生成器，让我们来看一个*“简单”*的例子，以理解*如何*编写一个递归生成器：

这个简短的函数——正如其名称所示——产生连续的二进制数字。当调用时，它首先简单地生成 `"1"`，然后进行递归调用。递归调用也生成 `"1"`，但这会作为 `prefix` 传递给之前的非递归调用。在计算出前缀后，非递归调用生成两个值 `"10"` 和 `"11"`。之后，递归调用继续执行，进行另一个递归调用，深入一层，循环继续——前缀向上冒泡，因此外部帧总是先以 `"0"` 结束，然后是 `"1"`。

现在，如果我们运行它，我们会得到：

当涉及递归时，仅仅解释代码并不足以真正理解发生了什么。所以，如果你不确定 `binary_counter` 实际上是如何工作的，那么我们来逐步演练一下：

上面的修改版本添加了一个 `depth` 参数和几个打印语句，以帮助演示代码的作用。如果我们现在调用这段代码，我们会得到以下结果：

我希望这能让事情变得更清晰一些，如果不行，可以考虑手动逐步演练，或者使用你选择的 IDE 中的调试器，以便实时查看堆栈帧和变量。

你可能也会问，*“以这种方式计算二进制数字有什么意义？”* —— 答案是，没有什么好的理由。确实有更好、更易读的方法来做这件事，但我认为它很好地演示了这个概念。话虽如此，让我们现在看看更多如何使用递归生成器的有用示例…

# 将其发挥到最佳使用

在递归的例子中，显而易见的候选者是各种数学函数，或者——如这里所示——组合数学，具体来说是 [幂集](https://en.wikipedia.org/wiki/Power_set)：

这里的函数使用了类似于之前二进制计数器的流程。为了更好地理解它，我们可以将递归部分翻译为：

+   对于更小的幂集 (`sequence[1:]`) 中的每个结果...

+   … 返回未使用的值 (`[sequence[0]]`) + 结果 (`item`)

+   … 然后仅返回结果 (`item`)

虽然数学函数可以通过递归很好的实现，但它们并不是我们日常使用的内容，所以现在我们来看看其他不同的东西：

上面的 `accumulate` 函数计算其列表参数的累积总和（和）。虽然上面的代码可以工作，但我不建议在实际中使用它，因为你可以且应该使用以下代码：

在讨论 `itertools` 的话题时，我们也来看看如何重新实现其他常见函数：

`flatten` 函数可以用来展开嵌套列表（或其他可迭代对象）。我在这里展示这个函数是因为它使用了与之前不同的流程——它利用 `try`/ `except` 来分离基本/非递归部分和递归代码。

然而，如果需要，也可以在没有 `try`/ `except` 的情况下重写：

说到递归时，我们显然要展示递归数据结构的示例，在这个案例中是二叉树：

上述代码实现了一个二叉树，包括`__iter__`方法中的递归生成器。`inorder`函数中也实现了相同的功能，使递归调用更加清晰。

为了展示上述代码的使用方法，让我们创建一个简单的树结构：

与（二叉）树的遍历类似，我们也可以在遍历 JSON 时使用递归生成器：

以这种方式遍历 JSON 可能是实用的，如果你处理的是非常大的数据，一次性加载会消耗大量内存。

到现在为止，你可能已经掌握了这些奇怪的生成器是如何工作的，但我们还是看看调用上述代码时会发生什么：

最后，另一个常常递归遍历的树状数据结构是文件树：

在这里我们实现了`get_paths`函数，该函数递归地生成指定路径中的所有文件。话虽如此，对于这个任务，使用内置的`path.rglob("*")`会更好，因为它也返回生成器。

此外，虽然在这个实例中不太有用，但值得注意的是，`send()`函数也可以与递归生成器一起使用。因此，上述函数的另一种实现方式：

这种生成器的风格在你需要控制递归或与协程通信时会很有用。

# 结论

我认为本文中的示例展示了许多可以递归表达的优雅解决方案。然而，优雅并不总是意味着更好。通常，使用不那么*“优雅”*或简洁的解决方案会产生更具可读性和普遍更好的代码。

所以，我们不要试图*“强行”*将递归生成器融入代码中，应该只在适当的情况下使用——也就是说——在实现从延迟评估中受益的递归函数时。

*这篇文章最初发布于* [*martinheinz.dev*](https://martinheinz.dev/blog/88)

[成为会员](https://medium.com/@martin.heinz/membership)并阅读 Medium 上的每一个故事。**你的会员费直接支持我和你阅读的其他作家。** 你还将获得 Medium 上每个故事的完全访问权限。

[](https://medium.com/@martin.heinz/membership?source=post_page-----9ee6af0dd803--------------------------------) [## 使用我的推荐链接加入 Medium - Martin Heinz

### 阅读 Martin Heinz（以及 Medium 上的其他数千名作家）的每一个故事。你的会员费直接支持…

medium.com](https://medium.com/@martin.heinz/membership?source=post_page-----9ee6af0dd803--------------------------------)

你可能还会喜欢…

[](/getting-started-with-google-apis-in-python-7f07b5d8b806?source=post_page-----9ee6af0dd803--------------------------------) ## 用 Python 入门 Google API

### 使用 Python 和 Google API 来自动化你在 Gmail、Google Drive、日历等上的所有操作的速成课程：

[towardsdatascience.com 在这里 [## 你可能没听说过的 Python 魔法方法]

### 你可能不知道的许多 Python 魔法方法 — 让我们来了解它们的功能以及如何使用它们…

towardsdatascience.com
