# 类型 I 和 II 错误以及假设检验中的样本量计算

> 原文：[https://towardsdatascience.com/type-i-ii-errors-and-sample-size-calculation-in-hypothesis-testing-760dae42a065?source=collection_archive---------3-----------------------#2023-02-23](https://towardsdatascience.com/type-i-ii-errors-and-sample-size-calculation-in-hypothesis-testing-760dae42a065?source=collection_archive---------3-----------------------#2023-02-23)

## 影响假设检验结果的因素

[](https://aaron-zhu.medium.com/?source=post_page-----760dae42a065--------------------------------)[![Aaron Zhu](../Images/42e9690c4b4aad63f396b171a74e29f7.png)](https://aaron-zhu.medium.com/?source=post_page-----760dae42a065--------------------------------)[](https://towardsdatascience.com/?source=post_page-----760dae42a065--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----760dae42a065--------------------------------) [Aaron Zhu](https://aaron-zhu.medium.com/?source=post_page-----760dae42a065--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffbd30d6294e5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-i-ii-errors-and-sample-size-calculation-in-hypothesis-testing-760dae42a065&user=Aaron+Zhu&userId=fbd30d6294e5&source=post_page-fbd30d6294e5----760dae42a065---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----760dae42a065--------------------------------) ·9 分钟阅读·Feb 23, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F760dae42a065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-i-ii-errors-and-sample-size-calculation-in-hypothesis-testing-760dae42a065&user=Aaron+Zhu&userId=fbd30d6294e5&source=-----760dae42a065---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F760dae42a065&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftype-i-ii-errors-and-sample-size-calculation-in-hypothesis-testing-760dae42a065&source=-----760dae42a065---------------------bookmark_footer-----------)![](../Images/7bb1699da57b004d15cbc9d02301f4be.png)

照片来源：[Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在统计学和数据分析的世界中，假设检验是一个基本概念，它在做出明智决策中扮演着至关重要的角色。在这篇博客中，我们将深入探讨[**假设检验**](https://medium.com/geekculture/what-is-hypothesis-testing-in-statistics-motivation-and-interpretation-39f8e63b1baf)，特别关注如何减少第一类和第二类错误。我们将讨论影响这些错误的因素，如显著性水平、样本大小和数据变异性。因此，让我们深入了解假设检验的复杂性吧！

我们将在整个博客中使用以下示例。

上学期的平均学生 GPA 是 2.70。当前学期推出了一个辅导计划。我们有兴趣进行以下假设检验，以研究该辅导计划是否能提高学生的 GPA。

在当前学期结束时，我们收集了 20 条随机 GPA 记录，并假设学生 GPA 服从标准差 (σ) 为 0.5 的正态分布。μ 代表总体的平均 GPA。

+   零假设：μ = 2.70（即，辅导计划对提高学生的 GPA 没有帮助。）

+   备择假设：μ > 2.70（即，辅导计划…）
