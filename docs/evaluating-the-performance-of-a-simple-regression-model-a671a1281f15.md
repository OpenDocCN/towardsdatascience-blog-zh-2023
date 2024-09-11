# 通过图表和指标评估简单回归模型的表现

> 原文：[`towardsdatascience.com/evaluating-the-performance-of-a-simple-regression-model-a671a1281f15?source=collection_archive---------1-----------------------#2023-05-27`](https://towardsdatascience.com/evaluating-the-performance-of-a-simple-regression-model-a671a1281f15?source=collection_archive---------1-----------------------#2023-05-27)

[](https://wendynavarrete.medium.com/?source=post_page-----a671a1281f15--------------------------------)![Wendy Navarrete](https://wendynavarrete.medium.com/?source=post_page-----a671a1281f15--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a671a1281f15--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a671a1281f15--------------------------------) [Wendy Navarrete](https://wendynavarrete.medium.com/?source=post_page-----a671a1281f15--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe036a91a10cb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-the-performance-of-a-simple-regression-model-a671a1281f15&user=Wendy+Navarrete&userId=e036a91a10cb&source=post_page-e036a91a10cb----a671a1281f15---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a671a1281f15--------------------------------) ·5 min read·2023 年 5 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa671a1281f15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-the-performance-of-a-simple-regression-model-a671a1281f15&user=Wendy+Navarrete&userId=e036a91a10cb&source=-----a671a1281f15---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa671a1281f15&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fevaluating-the-performance-of-a-simple-regression-model-a671a1281f15&source=-----a671a1281f15---------------------bookmark_footer-----------)![](img/ebc28a965f21b0cdabc575f539659231.png)

版权归 [Pixabay](https://pixabay.com) 所有

几年前，当我创建我的第一个简单线性回归模型时，我对如何评估我的模型，甚至是否是一个好的预测器，没有明确的概念。现在，我决定写这篇文章，集中讨论我用来确定简单线性回归模型优度的步骤。你将会看到对简单线性回归基础知识和假设的简要回顾，以及检查模型的待办事项清单。

# 什么是简单线性回归

这是最简单的回归分析模型，用于检查两个变量之间的关系：一个是因变量（也称为响应、目标或结果），另一个是自变量（也称为解释变量或预测变量）。该关系的估计可以在图中看到是一条直线。

简单回归的主要目标是：

+   识别变量之间的关系

+   基于单个自变量*x*预测因变量 Y

+   检验关联关系的假设

# 模型公式
