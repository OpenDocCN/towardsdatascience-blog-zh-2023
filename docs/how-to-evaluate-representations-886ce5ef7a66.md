# 如何评估表示

> 原文：[https://towardsdatascience.com/how-to-evaluate-representations-886ce5ef7a66?source=collection_archive---------7-----------------------#2023-09-14](https://towardsdatascience.com/how-to-evaluate-representations-886ce5ef7a66?source=collection_archive---------7-----------------------#2023-09-14)

## 从无监督到监督指标

[](https://medium.com/@mina.ghashami?source=post_page-----886ce5ef7a66--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----886ce5ef7a66--------------------------------)[](https://towardsdatascience.com/?source=post_page-----886ce5ef7a66--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----886ce5ef7a66--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----886ce5ef7a66--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-representations-886ce5ef7a66&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----886ce5ef7a66---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----886ce5ef7a66--------------------------------) ·8分钟阅读·2023年9月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F886ce5ef7a66&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-representations-886ce5ef7a66&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----886ce5ef7a66---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F886ce5ef7a66&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-evaluate-representations-886ce5ef7a66&source=-----886ce5ef7a66---------------------bookmark_footer-----------)![](../Images/92a89b397bf306df6fcef24bd78eb2a7.png)

版权：图片来自 unsplash.com

*嵌入*，也称为*表示*，是对实体（如单词、文档、产品等）的密集向量表示。它们旨在捕捉语义意义并突出实体之间的相似性。一组好的表示不仅应有效编码实体的基本特征，还应具有紧凑性、意义性以及在各种任务中的鲁棒性等特性。在本文中，我们将探讨各种评估指标来评估表示的质量。让我们开始吧。

# 一个评估框架

任何评估框架都由三个主要组件组成：

1.  **基准方法**：这作为一个基准，与新方法或模型进行比较。它为评估所提出方法的性能提供了一个参考点。

1.  **一组评估指标**：评估指标是用于量化评估模型性能的度量。这些指标可以是监督式或非监督式的，定义了输出成功的评估方式。

1.  **评估数据集**：评估数据集是用于评估模型性能的标注/注释或未标注的数据集合。这个数据集…
