# 使用非线性编程优化您的营销预算

> 原文：[`towardsdatascience.com/the-art-of-spending-optimizing-your-marketing-budget-with-nonlinear-optimization-6c8a39afb3c2?source=collection_archive---------4-----------------------#2023-05-22`](https://towardsdatascience.com/the-art-of-spending-optimizing-your-marketing-budget-with-nonlinear-optimization-6c8a39afb3c2?source=collection_archive---------4-----------------------#2023-05-22)

## CVXPY 简介：最大化营销投资回报

[](https://medium.com/@mlabonne?source=post_page-----6c8a39afb3c2--------------------------------)![Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----6c8a39afb3c2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6c8a39afb3c2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c8a39afb3c2--------------------------------) [Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----6c8a39afb3c2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc89da634938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-spending-optimizing-your-marketing-budget-with-nonlinear-optimization-6c8a39afb3c2&user=Maxime+Labonne&userId=dc89da634938&source=post_page-dc89da634938----6c8a39afb3c2---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6c8a39afb3c2--------------------------------) · 9 min 阅读 · 2023 年 5 月 22 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6c8a39afb3c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-spending-optimizing-your-marketing-budget-with-nonlinear-optimization-6c8a39afb3c2&user=Maxime+Labonne&userId=dc89da634938&source=-----6c8a39afb3c2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6c8a39afb3c2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-art-of-spending-optimizing-your-marketing-budget-with-nonlinear-optimization-6c8a39afb3c2&source=-----6c8a39afb3c2---------------------bookmark_footer-----------)![](img/9c6e46125f31204083c5d703d3c90cd5.png)

作者提供的图片

在数字营销时代，企业面临的挑战是如何将营销预算分配到多个渠道，以最大化销售。

然而，随着它们扩大影响力，这些公司不可避免地面临**收益递减**的问题——即对一个营销渠道的额外投资带来逐渐减少的转化增加。这就是营销预算分配的概念介入的地方，为整个过程增加了另一层复杂性。

在本文中，我们将深入探讨非线性编程的潜力，特别是圆锥优化（或锥编程），作为营销预算分配的工具。通过使用这种先进的数学技术，我们旨在优化各个平台上的营销预算分配，以提取最大价值和可能的最高投资回报率。

代码可以在 [GitHub](https://github.com/mlabonne/linear-programming-course/blob/main/4_Maximize_Your_Marketing_ROI_with_Nonlinear_Optimization.ipynb) 和 [Google Colab](https://colab.research.google.com/drive/1V7z8giemuTk92s_JMxIyr1Clr2TwY7xl?usp=sharing) 上获取。

# **💰 营销预算分配**

营销预算分配是任何广告活动的关键方面，需要企业在不同渠道间战略性地分配资源。目标是最大化…
