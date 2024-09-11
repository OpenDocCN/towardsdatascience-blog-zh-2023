# 假设检验与示例介绍

> 原文：[https://towardsdatascience.com/introduction-to-hypothesis-testing-with-examples-60a618fb1799?source=collection_archive---------2-----------------------#2023-01-05](https://towardsdatascience.com/introduction-to-hypothesis-testing-with-examples-60a618fb1799?source=collection_archive---------2-----------------------#2023-01-05)

## 一份关于假设检验的易懂指南，包含示例和可视化内容

[](https://ms-neerajkrishna.medium.com/?source=post_page-----60a618fb1799--------------------------------)[![Neeraj Krishna](../Images/7bea17130a09d3382cba3b12ca5e3d7b.png)](https://ms-neerajkrishna.medium.com/?source=post_page-----60a618fb1799--------------------------------)[](https://towardsdatascience.com/?source=post_page-----60a618fb1799--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----60a618fb1799--------------------------------) [Neeraj Krishna](https://ms-neerajkrishna.medium.com/?source=post_page-----60a618fb1799--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d6b9cde0656&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-hypothesis-testing-with-examples-60a618fb1799&user=Neeraj+Krishna&userId=8d6b9cde0656&source=post_page-8d6b9cde0656----60a618fb1799---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----60a618fb1799--------------------------------) ·8分钟阅读·2023年1月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F60a618fb1799&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-hypothesis-testing-with-examples-60a618fb1799&user=Neeraj+Krishna&userId=8d6b9cde0656&source=-----60a618fb1799---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F60a618fb1799&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-hypothesis-testing-with-examples-60a618fb1799&source=-----60a618fb1799---------------------bookmark_footer-----------)

我见过的大多数假设检验教程都从对分布的先验假设开始，列出一些定义和公式，然后直接应用它们解决问题。

然而，在本教程中，我们将从基本原理开始学习。这将是一个以示例为驱动的教程，我们将从一个基本示例入手，逐步深入了解假设检验的基础。

让我们开始吧。

# 你选择了哪个骰子？

![](../Images/b0ddcc52a615d3df8c01cd87ee086db8.png)

图片来源于 [Brett Jordan](https://unsplash.com/ja/@brett_jordan?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 想象一下你面前有两个无法区分的骰子。一个是公平的，另一个是加重的。你随机挑选一个骰子并掷它。在观察到它落在哪个面上后，你能确定你选择的是哪个骰子吗？

骰子的概率分布如下所示：

```py
**Die 1:**
P(X=x) = 1/6 if x = {1, 2, 3, 4, 5, 6}

**Die 2:**
P(X=x) = 1/4 if x = {1, 2}
       = 1/8 if x = {3, 4, 5, 6}
```

在二元假设检验问题中，我们通常会面临两个选择，我们称之为假设，我们需要决定选择其中一个。

假设由H₀和H₁表示，分别称为原假设和备择假设。在假设检验中，我们要么拒绝原假设，要么接受它。

在我们的例子中，骰子1和骰子2分别是原假设和备择假设。

如果你直观地考虑一下，如果骰子落在1或2上，更可能是骰子2，因为它更容易落在1或2上。因此，接受或拒绝原假设的决定依赖于观察值的分布。

因此，我们可以说假设检验的目标是画出一个边界，将观察空间分成两个区域：拒绝区和接受区。

![](../Images/8d5f436d057692a2eeeea483902637dd.png)

如果观察结果落在拒绝区，我们就会拒绝原假设，否则我们接受它。现在，决策边界并不会是完美的，我们会犯错误。例如，骰子1落在1或2上，我们可能会误认为是骰子2；但这种情况发生的概率较小。我们将在下一节学习如何计算错误的概率。

我们如何确定决策边界？有一种简单有效的方法叫做似然比检验，我们接下来将讨论。

# 似然比检验

你首先要意识到观察值的分布取决于假设。下面我绘制了在这两个假设下我们例子的分布：

![](../Images/e66604f6421d83c8cfe8d2f182dcbd29.png)

在两种假设下观察结果的概率分布

现在，`P(X=x;H₀)`和`P(X=x;H₁)`分别表示假设H₀和H₁下的*似然性*。它们的比率告诉我们*某个假设相对于另一个假设的可能性*。

这个比率称为似然比，表示为`L(X)`。`L(X)`是一个随机变量，依赖于观察值`x`。

![](../Images/0b6dfad392914d607c367f20c2fef996.png)

似然比

在似然比检验中，如果比率高于某个值，即`L(X) > 𝜉`，则我们拒绝原假设，否则接受它。𝜉被称为临界比率。

这就是我们如何绘制决策边界的方法：我们将似然比大于临界比率的观察值与其余观察值分开。

所以，形式为`{x | L(x) > 𝜉}`的观察值落入拒绝区，而其余的观察值则落入接受区。

让我们用掷骰子示例来说明。似然比可以计算为：

```py
L(X) = (1/4) / (1/6) = 3/2 if x = {1, 2}
     = (1/8) / (1/6) = 3/4 if x = {3, 4, 5, 6}
```

似然比的图示如下：

![](../Images/2fbdfa03000c5e2795b431f8255159ae.png)

现在决策边界的放置取决于选择临界比率。假设临界比率在 3/2 和 3/4 之间，即 `3/4 < 𝜉 < 3/2`。那么我们的决策边界看起来是这样的：

```py
if 3/4 < 𝜉 < 3/2:

L(X) > 𝜉 if x = {1, 2} (rejection region)
L(X) < 𝜉 if x = {3, 4, 5, 6} (acceptance region)
```

![](../Images/95318d6315a49aef05a264551487e979.png)

如果似然比在 3/4 和 3/2 之间，则拒绝和接受区域

让我们讨论与此决策相关的错误。第一种错误发生在观察 *x* 属于拒绝区域但发生在零假设下。在我们的示例中，这意味着骰子 1 落在 1 或 2 上。

这被称为错误拒绝或类型 1 错误。此错误的概率用 `𝛼` 表示，可以计算为：

```py
False Rejection Error:

𝛼 = P(X|L(X) > 𝜉 ; H₀)
```

第二种错误发生在观察 *x* 属于接受区域但发生在备择假设下。这被称为错误接受或类型 2 错误。此错误的概率用 `𝛽` 表示，可以计算为：

```py
False Acceptance Error:

𝛽 = P(X|L(X) < 𝜉 ; H₁)
```

在我们的示例中，错误拒绝和错误接受可以计算如下：

```py
Computing errors in the dice example:

𝛼 = P(X|L(X) > 𝜉 ; H₀)
  = P(X={1, 2} ; H₀)
  = 2 * 1/6 
  = 1/3

𝛽 = P(X|L(X) < 𝜉 ; H₁)
  = P(X={3, 4, 5, 6} ; H₁)
  = 4 * 1/8
  = 1/2
```

让我们考虑两种其他情况，其中临界比率取以下值：`𝜉 > 3/2` 和 `𝜉 < 3/4`。

![](../Images/5e2eee010d489c01b4724221afae7208.png)

临界比率 < 3/4

![](../Images/bab18d76a6311fca5ef66ee2705ef8c4.png)

临界比率 > 3/2

类型 1 和类型 2 错误可以类似地计算。

```py
𝛼 = 0 if 𝜉 > 3/2
  = 1/3 if 3/4 < 𝜉 < 3/2
  = 1 if 𝜉 < 3/4

𝛽 = 1 if 𝜉 > 3/2
  = 1/2 if 3/4 < 𝜉 < 3/2
  = 0 if 𝜉 < 3/4
```

让我们绘制不同 𝜉 值下的两种错误。

![](../Images/9977f5a8ac232948b631d4bbc90dbc94.png)

随着临界值 𝜉 的增加，拒绝区域变得更小。因此，错误拒绝概率 𝛼 减少，而错误接受概率 𝛽 增加。

# 似然比检验提供了最小的错误

我们可以在观察空间中的任何地方绘制边界。为什么我们需要计算似然比并经历所有这些呢？让我们来看看原因。

下面我计算了不同边界下的类型 I 和类型 II 错误。

```py
Type I and Type II errors for different boundaries.

'|' is the separator - {rejection region | acceptance region}

1\. {|, 1, 2, 3, 4, 5, 6}
𝛼 = P(x={} ; H₀) = 0
𝛽 = P(x={1, 2, 3, 4, 5, 6} ; H₁) = 1
𝛼 + 𝛽 = 1

2\. {1, |, 2, 3, 4, 5, 6}
𝛼 = P(x={1} ; H₀) = 1/6
𝛽 = P(x={2, 3, 4, 5, 6} ; H₁) = 1/4 + 1/2 = 3/4
𝛼 + 𝛽 = 0.916

3\. {1, 2, |, 3, 4, 5, 6}
𝛼 = P(x={1, 2} ; H₀) = 1/3
𝛽 = P(x={3, 4, 5, 6} ; H₁) = 1/2
𝛼 + 𝛽 = 0.833

4\. {1, 2, 3, |, 4, 5, 6}
𝛼 = P(x={1, 2, 3} ; H₀) = 1/2
𝛽 = P(x={4, 5, 6} ; H₁) = 3/8
𝛼 + 𝛽 = 0.875

5\. {1, 2, 3, 4, |, 5, 6}
𝛼 = P(x={1, 2, 3, 4} ; H₀) = 2/3
𝛽 = P(x={5, 6} ; H₁) = 1/4
𝛼 + 𝛽 = 0.916

6\. {1, 2, 3, 4, 5, |, 6}
𝛼 = P(x={1, 2, 3, 4, 5} ; H₀) = 5/6
𝛽 = P(x={6} ; H₁) = 1/8
𝛼 + 𝛽 = 0.958

6\. {1, 2, 3, 4, 5, 6, |}
𝛼 = P(x={1, 2, 3, 4, 5, 6} ; H₀) = 1
𝛽 = P(x={} ; H₁) = 0
𝛼 + 𝛽 = 1
```

不同边界下的类型 I 和类型 II 错误及其总和的图示如下：

![](../Images/77a01f393e24d5ec68b1b7d246a65acf.png)

我们可以看到，对于从似然比检验获得的最优临界比率值，类型 I 和类型 II 错误的总和是最小的。

换句话说，对于给定的错误拒绝概率，似然比检验提供了最小的错误接受概率。

这被称为 Neyman-Pearson 引理。我在文章末尾引用了理论证明。

# 连续分布的似然比检验

在上述示例中，我们没有讨论如何选择临界比率 𝜉 的值。概率分布是离散的，因此临界比率 𝜉 的小变化不会影响边界。

当处理连续分布时，我们固定错误拒绝概率 𝛼 的值，并基于此计算临界比率。

```py
P(L(X) > 𝜉 ; H₀) = 𝛼
```

不过，过程将是相同的。一旦我们获得了临界比率的值，我们就会划分观察空间。

对于 𝛼，典型的选择有 `𝛼 = 0.01`、`𝛼 = 0.05` 或 `𝛼 = 0.01`，具体取决于假设检验中错误拒绝的不可接受程度。

![](../Images/09b7cae1d0096f945e43629631ba9316.png)

例如，如果我们处理的是正态分布，我们可以将其标准化，然后查找 Z 表以找到给定 𝛼 的 𝜉。

# 结论

在这篇文章中，我们探讨了假设检验的理念和过程背后的直觉。整个过程可以在下面的图示中总结：

![](../Images/9d61d8dadf632e83cdbace6d1815e5f0.png)

我们从两个假设 H₀ 和 H₁ 开始，使得基础数据的分布依赖于这些假设。目标是通过找到一个决策规则，将观测值 *x* 映射到两个假设之一，从而证明或推翻原假设 H₀。最后，我们计算与决策规则相关的错误。

然而，在现实世界中，这两种假设之间的区别不会那么直接。因此，我们必须做一些变通处理以进行假设检验。让我们在下一篇文章中讨论这个问题。

希望你喜欢这篇文章。让我们保持联系。

```py
**Let's Connect** Hope you've enjoyed the article. Please clap and follow if you did.

You can also reach out to me on [LinkedIn](https://www.linkedin.com/in/neerajkrishnadev/) and [Twitter](https://twitter.com/WingedRasengan).
```

# 图像和图表来源

本文中的所有图像、图表和示意图均由作者创建；除非在说明中明确提到。

# 参考文献

[《Dimitri Bertsekas 和 John Tsitsiklis 的概率论导论》第9章第3节](https://www.amazon.in/Introduction-Probability-Dimitri-P-Bertsekas/dp/188652923X)
