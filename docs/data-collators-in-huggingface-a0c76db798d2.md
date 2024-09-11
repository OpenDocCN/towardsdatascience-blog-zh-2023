# HuggingFace 中的数据收集器

> 原文：[https://towardsdatascience.com/data-collators-in-huggingface-a0c76db798d2?source=collection_archive---------3-----------------------#2023-11-08](https://towardsdatascience.com/data-collators-in-huggingface-a0c76db798d2?source=collection_archive---------3-----------------------#2023-11-08)

## 它们是什么以及它们的作用

[](https://medium.com/@mina.ghashami?source=post_page-----a0c76db798d2--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----a0c76db798d2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0c76db798d2--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0c76db798d2--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----a0c76db798d2--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-collators-in-huggingface-a0c76db798d2&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----a0c76db798d2---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0c76db798d2--------------------------------) ·9 min read·Nov 8, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0c76db798d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-collators-in-huggingface-a0c76db798d2&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----a0c76db798d2---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0c76db798d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-collators-in-huggingface-a0c76db798d2&source=-----a0c76db798d2---------------------bookmark_footer-----------)![](../Images/2ddfbf36b4be5c6b3f2d3216d22460d6.png)

来自 [unsplash.com](https://unsplash.com/photos/assorted-source-codes-sp1BZ1atp7M) 的图像

当我开始学习 HuggingFace 时，数据收集器是我理解最困难的组件之一。我很难理解它们，也找不到足够好的资源来直观地解释它们。

在本文中，我们将看看数据收集器是什么，它们如何不同，以及如何编写自定义数据收集器。

# 数据收集器：高级介绍

数据收集器在 HuggingFace 中是数据处理的重要部分。在对数据进行标记化之后，我们都会使用它们，并在将数据传递给训练模型的 Trainer 对象之前使用它们。

> 简而言之，它们将一系列样本汇总成一个小型训练批次。它们的具体操作取决于它们所定义的任务，但至少会对输入样本进行填充或截断，以确保所有样本在一个小批次中具有相同的长度。典型的小批量大小范围从16到256个样本，具体取决于模型大小、数据和硬件限制。

数据整理器是**任务特定**的。每个任务都有一个数据整理器：

+   因果语言建模（CLM）

+   掩蔽语言模型（MLM）

+   序列分类
