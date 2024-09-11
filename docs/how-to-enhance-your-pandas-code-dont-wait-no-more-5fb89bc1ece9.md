# 如何提升你的 Pandas 代码——不要再等待了

> 原文：[`towardsdatascience.com/how-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9?source=collection_archive---------16-----------------------#2023-04-10`](https://towardsdatascience.com/how-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9?source=collection_archive---------16-----------------------#2023-04-10)

## 6 个简单的改变来提高你 Pandas 代码的性能

[](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)![Pol Marin](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----5fb89bc1ece9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----5fb89bc1ece9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5fb89bc1ece9--------------------------------) ·9 分钟阅读·2023 年 4 月 10 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5fb89bc1ece9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-enhance-your-pandas-code-dont-wait-no-more-5fb89bc1ece9&source=-----5fb89bc1ece9---------------------bookmark_footer-----------)![](img/5316eff4e5a960cbcac4b7ab3908130b.png)

图片由 [Pascal Müller](https://unsplash.com/@millerthachiller?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**Pandas** 可以说是任何数据科学家组合中的必备工具。无论你是否使用它，掌握它始终是一个资产。

然而，就像其他任何事情一样，关键不在于知道如何使用*pandas*，而在于实际编写高效的*pandas*代码。这就是大多数人不擅长的地方，但在阅读完这篇文章后，你将不再是其中的一员。

但，首先要做的事。

## 什么是 Pandas，效率为什么重要？

根据官方定义，“**pandas** 是一个快速、强大、灵活且易于使用的开源数据分析和处理工具，建立在[Python](https://www.python.org/)编程语言之上”[1]。

这确实非常强大，是我们在想要处理数据时的默认选择。

额外说明一下，它将数据集加载到我们的 RAM 中，因此优化其空间使用显得非常重要。

但我们不仅仅想优化其内存管理，我们可能更关心的是让代码执行更快。为什么？显而易见，更快的代码能节省时间。而且你知道他们怎么说：时间就是金钱（尤其是如果…
