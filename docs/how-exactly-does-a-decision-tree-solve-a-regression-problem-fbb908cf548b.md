# 决策树如何解决回归问题？

> 原文：[`towardsdatascience.com/how-exactly-does-a-decision-tree-solve-a-regression-problem-fbb908cf548b?source=collection_archive---------3-----------------------#2023-11-25`](https://towardsdatascience.com/how-exactly-does-a-decision-tree-solve-a-regression-problem-fbb908cf548b?source=collection_archive---------3-----------------------#2023-11-25)

## 从头开始在 Python 中构建你自己的决策树回归器，并揭示其背后的工作原理

[](https://medium.com/@gurjinderkaur95?source=post_page-----fbb908cf548b--------------------------------)![Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----fbb908cf548b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fbb908cf548b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fbb908cf548b--------------------------------) [Gurjinder Kaur](https://medium.com/@gurjinderkaur95?source=post_page-----fbb908cf548b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F79ee1ef48e0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-exactly-does-a-decision-tree-solve-a-regression-problem-fbb908cf548b&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=post_page-79ee1ef48e0c----fbb908cf548b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fbb908cf548b--------------------------------) ·12 分钟阅读·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffbb908cf548b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-exactly-does-a-decision-tree-solve-a-regression-problem-fbb908cf548b&user=Gurjinder+Kaur&userId=79ee1ef48e0c&source=-----fbb908cf548b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffbb908cf548b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-exactly-does-a-decision-tree-solve-a-regression-problem-fbb908cf548b&source=-----fbb908cf548b---------------------bookmark_footer-----------)![](img/79b7360f9005933ba478d9db039fa1bc.png)

图片由 [Chris Lawton](https://unsplash.com/@chrislawton?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我将通过一个简单的例子、流程图和代码演示决策树回归器（*也称为回归树*）的底层逻辑。在阅读之后，你将能够清晰理解回归树的工作原理，并在下一个回归挑战中更加周到和自信地使用和调整它们。

我们将涵盖以下内容：

+   对决策树的精彩介绍

+   生成一个用于训练回归树的玩具数据集

+   以流程图的形式概述回归树的逻辑

+   参考流程图使用 NumPy 和 Pandas 编写代码并进行第一次拆分

+   使用 Plotly 在第一次拆分后可视化决策树

+   使用递归来泛化代码，构建整个回归树

+   使用 scikit-learn 执行相同的任务并比较结果（*剧透：你会为自己生成与 scikit-learn 相同的输出而感到自豪！*）
