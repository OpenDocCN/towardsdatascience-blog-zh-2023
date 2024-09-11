# 如何生成和评估知识图谱嵌入的性能？

> 原文：[https://towardsdatascience.com/how-to-generate-and-evaluate-the-performance-of-knowledge-graph-embeddings-95789abcb0c1?source=collection_archive---------8-----------------------#2023-04-10](https://towardsdatascience.com/how-to-generate-and-evaluate-the-performance-of-knowledge-graph-embeddings-95789abcb0c1?source=collection_archive---------8-----------------------#2023-04-10)

## 知识图谱嵌入训练和评估的详细指南

[](https://medium.com/@rohithtejam?source=post_page-----95789abcb0c1--------------------------------)[![Rohith Teja](../Images/3b83438350f3eb69309b9abf715d6ee7.png)](https://medium.com/@rohithtejam?source=post_page-----95789abcb0c1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----95789abcb0c1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----95789abcb0c1--------------------------------) [Rohith Teja](https://medium.com/@rohithtejam?source=post_page-----95789abcb0c1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd66134dd9f0d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-and-evaluate-the-performance-of-knowledge-graph-embeddings-95789abcb0c1&user=Rohith+Teja&userId=d66134dd9f0d&source=post_page-d66134dd9f0d----95789abcb0c1---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----95789abcb0c1--------------------------------) ·7分钟阅读·2023年4月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F95789abcb0c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-and-evaluate-the-performance-of-knowledge-graph-embeddings-95789abcb0c1&user=Rohith+Teja&userId=d66134dd9f0d&source=-----95789abcb0c1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F95789abcb0c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-generate-and-evaluate-the-performance-of-knowledge-graph-embeddings-95789abcb0c1&source=-----95789abcb0c1---------------------bookmark_footer-----------)![](../Images/d8b56187761abee2d42e0d8a1cba1a51.png)

图片由 [Alina Grubnyak](https://unsplash.com/it/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/network?orientation=landscape&utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在你开始阅读这篇文章之前，我假设你对知识图谱嵌入有一些基本了解。如果没有，我建议你查看以下文章，它能给你对这一概念一个不错的介绍：

[](/knowledge-graph-embedding-a-simplified-version-e6b0a03d373d?source=post_page-----95789abcb0c1--------------------------------) [## 知识图谱嵌入—简化版！

### 解释知识图谱嵌入到底是什么以及如何计算它们。

towardsdatascience.com](/knowledge-graph-embedding-a-simplified-version-e6b0a03d373d?source=post_page-----95789abcb0c1--------------------------------)

现在我们已经处理完这些内容，本文将展示如何生成知识图谱嵌入，解释它们，并评估它们在基于图的任务中的表现。

# 知识图谱

考虑一个[国家](https://pykeen.readthedocs.io/en/stable/api/pykeen.datasets.Countries.html#pykeen.datasets.Countries)的知识图谱。在这个图谱中，我们将国家和地区的名称作为实体，关系由两个属性表示，即“邻居”和“位于”。

> 这个图谱是PyKEEN Python库中的内置数据集。

查看以下作为数据框显示的知识图谱三元组的截图：
