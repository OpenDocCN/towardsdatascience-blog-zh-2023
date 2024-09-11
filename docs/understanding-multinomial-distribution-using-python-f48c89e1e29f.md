# 使用 Python 理解多项分布

> 原文：[https://towardsdatascience.com/understanding-multinomial-distribution-using-python-f48c89e1e29f?source=collection_archive---------9-----------------------#2023-01-06](https://towardsdatascience.com/understanding-multinomial-distribution-using-python-f48c89e1e29f?source=collection_archive---------9-----------------------#2023-01-06)

## 多项分布背后的数学和直觉

[](https://reza-bagheri79.medium.com/?source=post_page-----f48c89e1e29f--------------------------------)[![Reza Bagheri](../Images/7c5a7dc9e6e31048ce31c8d49055987c.png)](https://reza-bagheri79.medium.com/?source=post_page-----f48c89e1e29f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f48c89e1e29f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f48c89e1e29f--------------------------------) [Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----f48c89e1e29f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fda2d000eaa4d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-multinomial-distribution-using-python-f48c89e1e29f&user=Reza+Bagheri&userId=da2d000eaa4d&source=post_page-da2d000eaa4d----f48c89e1e29f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f48c89e1e29f--------------------------------) ·20 min 阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff48c89e1e29f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-multinomial-distribution-using-python-f48c89e1e29f&user=Reza+Bagheri&userId=da2d000eaa4d&source=-----f48c89e1e29f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff48c89e1e29f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funderstanding-multinomial-distribution-using-python-f48c89e1e29f&source=-----f48c89e1e29f---------------------bookmark_footer-----------)![](../Images/be421088ce08f6be5825b9f3e0a934fa.png)

来源: [https://pixabay.com/vectors/dice-game-die-luck-random-numbers-151867/](https://pixabay.com/vectors/dice-game-die-luck-random-numbers-151867/)

多项分布是二项分布的推广，用于计算具有多个结果的实验的概率。本文提供了对多项分布的直观介绍，并讨论了其数学性质。此外，文章将教你如何使用 Python 中的 SciPy 库来建模和可视化多项分布。

**二项分布**

由于多项式分布是二项分布的推广，我们将在这里简要回顾一下。在 [另一篇文章](https://medium.com/towards-data-science/understanding-probability-distributions-using-python-9eca9c1d9d38) 中，我更详细地讨论了单变量概率分布，所以如果你对二项分布或像随机变量和概率质量函数（PMF）这样的概念不太熟悉，我建议你先阅读 [那篇文章](https://medium.com/towards-data-science/understanding-probability-distributions-using-python-9eca9c1d9d38)。

*随机变量* 是一个其值由随机实验的结果决定的变量。随机变量通常用大写字母表示，但我们用小写字母来表示它可能取的特定值。例如，我们可以定义一个随机变量 *X* 来表示掷硬币的结果。为了做到这一点，我们需要为每个结果分配一个数值。我们可以表示…
