# 无监督学习系列 — 探索自组织映射

> 原文：[https://towardsdatascience.com/unsupervised-learning-series-exploring-self-organizing-maps-fe2efde9f7a1?source=collection_archive---------3-----------------------#2023-08-06](https://towardsdatascience.com/unsupervised-learning-series-exploring-self-organizing-maps-fe2efde9f7a1?source=collection_archive---------3-----------------------#2023-08-06)

## 了解自组织映射的工作原理以及它们为什么是有用的无监督学习算法

[](https://ivopbernardo.medium.com/?source=post_page-----fe2efde9f7a1--------------------------------)[![Ivo Bernardo](../Images/39887b6f3e63a67c0545e87962ad5df0.png)](https://ivopbernardo.medium.com/?source=post_page-----fe2efde9f7a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe2efde9f7a1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe2efde9f7a1--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----fe2efde9f7a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74eec53531c0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-self-organizing-maps-fe2efde9f7a1&user=Ivo+Bernardo&userId=74eec53531c0&source=post_page-74eec53531c0----fe2efde9f7a1---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe2efde9f7a1--------------------------------) ·16分钟阅读·2023年8月6日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe2efde9f7a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-self-organizing-maps-fe2efde9f7a1&user=Ivo+Bernardo&userId=74eec53531c0&source=-----fe2efde9f7a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe2efde9f7a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funsupervised-learning-series-exploring-self-organizing-maps-fe2efde9f7a1&source=-----fe2efde9f7a1---------------------bookmark_footer-----------)![](../Images/5f541e88bf23d4f4e488dda83aa7536d.png)

图片由 [teckhonc](https://unsplash.com/pt-br/@teckhonc) 提供 @Unsplash.com

**自组织映射（SOMs）是一种用于聚类** 和 **高维数据可视化的无监督神经网络**。SOMs 通过竞争学习算法进行训练，其中网络中的节点（也称为神经元）竞争代表输入数据的权利。

SOM架构由一个二维节点网格组成，每个节点都与一个权重向量相关，该向量代表SOM解决方案中质心的均值。节点的组织方式使得相似数据点周围的节点被组织在一起，从而生成一个表示底层数据的层。

SOM通常用于各种任务，例如：

+   数据可视化

+   异常检测

+   特征提取

+   聚类

**我们还可以将SOM可视化为最简单的神经网络版本，用于无监督学习！**

尽管它们开始时可能看起来令人困惑，但自组织映射（或称科霍宁映射，因其[发明者](https://en.wikipedia.org/wiki/Teuvo_Kohonen)命名）是一种有趣的算法类型，能够映射……
