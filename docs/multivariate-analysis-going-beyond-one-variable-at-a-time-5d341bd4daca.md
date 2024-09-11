# 多变量分析——超越单一变量

> 原文：[https://towardsdatascience.com/multivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca?source=collection_archive---------2-----------------------#2023-01-12](https://towardsdatascience.com/multivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca?source=collection_archive---------2-----------------------#2023-01-12)

## Python中的多变量分析与可视化

[](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)[![Farzad Mahmoodinobar](../Images/2d75209693b712300e6f0796bd2487d0.png)](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------) [Farzad Mahmoodinobar](https://medium.com/@fmnobar?source=post_page-----5d341bd4daca--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3c56b7d4893e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=post_page-3c56b7d4893e----5d341bd4daca---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5d341bd4daca--------------------------------) ·9 min read·2023年1月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5d341bd4daca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca&user=Farzad+Mahmoodinobar&userId=3c56b7d4893e&source=-----5d341bd4daca---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5d341bd4daca&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultivariate-analysis-going-beyond-one-variable-at-a-time-5d341bd4daca&source=-----5d341bd4daca---------------------bookmark_footer-----------)![](../Images/5c2d26281810d576715f7708bbbd9c84.png)

一只反思数据的猫头鹰，作者：[DALL.E 2](https://openai.com/dall-e-2/)

如今，企业和公司收集尽可能多的信息已经成为一种常见做法，即使这些数据在收集时的具体使用情况尚不明确——希望将来能够理解和利用这些数据。一旦这些数据集可用，数据驱动的个人将深入数据中寻找隐藏的模式和关系。多变量分析就是发现数据中隐藏模式的工具之一。

*多变量*分析涉及分析多个变量（即多变量数据）之间的关系，并理解它们如何相互影响。这是一个重要的工具，有助于我们更好地理解复杂的数据集，以做出数据驱动的和明智的决策。如果你只对分析单一变量的影响感兴趣，可以通过*单变量*分析来实现，相关内容我在[这篇文章](/univariate-analysis-intro-and-implementation-b9d1e07e5c16)中已经介绍过。
