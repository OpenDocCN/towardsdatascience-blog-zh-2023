# 如何创建有价值的数据测试

> 原文：[`towardsdatascience.com/how-to-create-valuable-data-tests-850e778718e1?source=collection_archive---------3-----------------------#2023-07-03`](https://towardsdatascience.com/how-to-create-valuable-data-tests-850e778718e1?source=collection_archive---------3-----------------------#2023-07-03)

## 重要的不是数量，而是质量。

[](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)![Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------) [Xiaoxu Gao](https://medium.com/@xiaoxugao?source=post_page-----850e778718e1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2adc5a07e772&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-valuable-data-tests-850e778718e1&user=Xiaoxu+Gao&userId=2adc5a07e772&source=post_page-2adc5a07e772----850e778718e1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----850e778718e1--------------------------------) ·9 分钟阅读·2023 年 7 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F850e778718e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-valuable-data-tests-850e778718e1&user=Xiaoxu+Gao&userId=2adc5a07e772&source=-----850e778718e1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F850e778718e1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-create-valuable-data-tests-850e778718e1&source=-----850e778718e1---------------------bookmark_footer-----------)![](img/a6433b06eebe7358fd99a93d34ff62f8.png)

照片由 [Shubham Dhage](https://unsplash.com/@theshubhamdhage) 提供，发布在 [Unsplash](https://unsplash.com/)

数据质量在过去一年中被广泛讨论。数据合同、数据产品和数据可观察性工具的逐渐采用无疑显示了数据从业者致力于为消费者提供高质量数据的决心。我们都喜欢看到这一点！

数据解决方案中的一个基本组成部分是数据测试。这是验证数据质量的最基本且实用的方法之一，并且在许多数据解决方案中明确或隐含地嵌入其中。

尽管其有效性为数据团队带来了显著的好处，但也引发了如何最大化其潜在价值的问题，因为更多的测试并不一定意味着更高的数据质量。在本文中，我想向你展示一些设计数据测试的方法。希望这些方法能为你提供一些启示。

> 值得注意的是，建议你结合这些方法，找到最适合你的平衡点。

## 质量 > 数量

![](img/50cbde299cf98bb64a915f7b5a14d136.png)

质量 > 数量（作者创建）
