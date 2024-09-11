# 使用 Python 量化肥尾的 4 种方法

> 原文：[`towardsdatascience.com/4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1?source=collection_archive---------4-----------------------#2023-12-07`](https://towardsdatascience.com/4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1?source=collection_archive---------4-----------------------#2023-12-07)

## 直觉和示例代码

[](https://shawhin.medium.com/?source=post_page-----10ce62c0ada1--------------------------------)![Shaw Talebi](https://shawhin.medium.com/?source=post_page-----10ce62c0ada1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----10ce62c0ada1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----10ce62c0ada1--------------------------------) [Shaw Talebi](https://shawhin.medium.com/?source=post_page-----10ce62c0ada1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff3998e1cd186&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1&user=Shaw+Talebi&userId=f3998e1cd186&source=post_page-f3998e1cd186----10ce62c0ada1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----10ce62c0ada1--------------------------------) · 11 分钟阅读 · 2023 年 12 月 7 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F10ce62c0ada1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F4-ways-to-quantify-fat-tails-with-python-10ce62c0ada1&source=-----10ce62c0ada1---------------------bookmark_footer-----------)![](img/b5203c61a2cee6046950ad6cec930138.png)

一条肥大的（猫的）尾巴。图片来自 Canva。

这是关于 [幂律和肥尾](https://pareto-power-laws-and-fat-tails-0355a187ee6a) 系列文章中的第三篇。在 [上一篇文章](https://medium.com/towards-data-science/detecting-power-laws-in-real-world-data-with-python-b464190fade6) 中，我们探讨了如何从实证数据中检测幂律。虽然这项技术很有用，但肥尾超越了简单地将数据拟合到幂律分布中。在这篇文章中，我将详细介绍 4 种量化肥尾的方法，并分享分析实际数据的示例 Python 代码。

*注意：如果你对“幂律分布”或“胖尾”这些术语不太熟悉，请参考* [*这篇文章*](https://medium.com/towards-data-science/pareto-power-laws-and-fat-tails-0355a187ee6a) *作为入门材料。*

在本系列的第一篇文章中，我们介绍了**胖尾**的概念，这描述了**稀有事件在分布的总体统计中所占的程度**。我们通过帕累托分布看到了胖尾的一个极端例子，例如，80%的销售额由 20%的客户产生（而 50%的销售额由仅 1%的客户产生）。
