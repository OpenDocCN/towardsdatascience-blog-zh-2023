# 采样方法简介

> 原文：[`towardsdatascience.com/introduction-to-sampling-methods-c934b64b6b08?source=collection_archive---------16-----------------------#2023-01-10`](https://towardsdatascience.com/introduction-to-sampling-methods-c934b64b6b08?source=collection_archive---------16-----------------------#2023-01-10)

## 在 Python 中实现逆变换采样、拒绝采样和重要性采样

[](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)![Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------) [Oliver S](https://medium.com/@hrmnmichaels?source=post_page-----c934b64b6b08--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff2daf6260cca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-sampling-methods-c934b64b6b08&user=Oliver+S&userId=f2daf6260cca&source=post_page-f2daf6260cca----c934b64b6b08---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----c934b64b6b08--------------------------------) ·8 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc934b64b6b08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-sampling-methods-c934b64b6b08&user=Oliver+S&userId=f2daf6260cca&source=-----c934b64b6b08---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc934b64b6b08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-sampling-methods-c934b64b6b08&source=-----c934b64b6b08---------------------bookmark_footer-----------)![](img/08c4a7efaed06bcd1a42a2a74b87d3d5.png)

由[Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄于[Unsplash](https://unsplash.com/photos/uKlneQRwaxY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我们将讨论如何从概率分布中进行采样。这是一个常见的需求，在机器学习的环境中，这通常用于运行概率模型的推断。然而，由于分布非常复杂，这通常是不可处理的。因此，这篇文章的主要重点是介绍解决此任务的近似方法，特别是使用数值采样，即[蒙特卡罗方法](https://en.wikipedia.org/wiki/Monte_Carlo_method)。

然而，为了介绍的目的，我们首先介绍逆变换抽样，它允许对任意可处理的分布进行精确推断。然后，我们将把重点转移到近似方法上，允许对（近乎）任意分布进行抽样或矩估计：我们首先介绍拒绝抽样，然后转向重要性抽样。

请注意，本文是让读者熟悉[1]系列文章中的第一篇，本文特别涵盖了第十一章的部分内容：抽样方法。

# 逆变换抽样

逆变换抽样方法允许从我们知道如何计算累积分布函数（[累积分布函数](https://en.wikipedia.org/wiki/Cumulative_distribution_function)）的任何分布中进行抽样。它包括对`y`进行抽样……
