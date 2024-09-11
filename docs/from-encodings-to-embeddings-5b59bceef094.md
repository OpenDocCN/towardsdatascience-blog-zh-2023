# 从编码到嵌入

> 原文：[https://towardsdatascience.com/from-encodings-to-embeddings-5b59bceef094?source=collection_archive---------3-----------------------#2023-09-07](https://towardsdatascience.com/from-encodings-to-embeddings-5b59bceef094?source=collection_archive---------3-----------------------#2023-09-07)

## **概念与基础：从 SVD 到神经网络**

[](https://medium.com/@mina.ghashami?source=post_page-----5b59bceef094--------------------------------)[![Mina Ghashami](../Images/745f53b94f5667a485299b49913c7a21.png)](https://medium.com/@mina.ghashami?source=post_page-----5b59bceef094--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5b59bceef094--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5b59bceef094--------------------------------) [Mina Ghashami](https://medium.com/@mina.ghashami?source=post_page-----5b59bceef094--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc99ed9ed7b9a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-encodings-to-embeddings-5b59bceef094&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=post_page-c99ed9ed7b9a----5b59bceef094---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5b59bceef094--------------------------------) ·16分钟阅读·2023年9月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5b59bceef094&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-encodings-to-embeddings-5b59bceef094&user=Mina+Ghashami&userId=c99ed9ed7b9a&source=-----5b59bceef094---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5b59bceef094&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-encodings-to-embeddings-5b59bceef094&source=-----5b59bceef094---------------------bookmark_footer-----------)![](../Images/ea2a97bef9136382285c2f7b5a61786a.png)

版权： [https://unsplash.com/](https://unsplash.com/)

在本文中，我们将讨论数据表示和机器学习领域中的两个基本概念：**编码** 和 **嵌入**。本文的内容部分来源于我在 [斯坦福大学 CS246 大规模数据集挖掘（MMDS）课程](https://web.stanford.edu/class/cs246/) 的讲座。希望对你有所帮助。

# 介绍

所有机器学习（ML）方法都处理输入特征向量，几乎所有这些方法都要求输入特征为 *数值型*。从 ML 的角度来看，特征有四种类型：

1.  *数值型（连续型或离散型）*：数值数据可以通过连续数据或离散数据来表征。连续数据可以在一个范围内取任何值，而离散数据具有明确的值。连续数值变量的例子是 *`height`*，离散数值变量的例子是 *`age`*。

1.  *分类数据（有序或无序）*：分类数据表示特征，例如眼睛颜色和家乡。分类数据可以是有序的或无序的。在有序变量中，数据落入按某种特定方式排序的类别中。例如，*`skill level`* 的值包括 *[`beginner`, `intermediate`, `advanced`]*。无序变量的值之间没有顺序。
