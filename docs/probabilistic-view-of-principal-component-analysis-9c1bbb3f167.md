# 主成分分析的概率视角

> 原文：[https://towardsdatascience.com/probabilistic-view-of-principal-component-analysis-9c1bbb3f167?source=collection_archive---------8-----------------------#2023-07-12](https://towardsdatascience.com/probabilistic-view-of-principal-component-analysis-9c1bbb3f167?source=collection_archive---------8-----------------------#2023-07-12)

## 潜在变量、期望最大化与变分推断

[](https://saptashwa.medium.com/?source=post_page-----9c1bbb3f167--------------------------------)[![Saptashwa Bhattacharyya](../Images/b01238113a1f6b91cb6fb0fbfa50303a.png)](https://saptashwa.medium.com/?source=post_page-----9c1bbb3f167--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9c1bbb3f167--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9c1bbb3f167--------------------------------) [Saptashwa Bhattacharyya](https://saptashwa.medium.com/?source=post_page-----9c1bbb3f167--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9a3c3c477239&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-view-of-principal-component-analysis-9c1bbb3f167&user=Saptashwa+Bhattacharyya&userId=9a3c3c477239&source=post_page-9a3c3c477239----9c1bbb3f167---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9c1bbb3f167--------------------------------) ·9分钟阅读·2023年7月12日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9c1bbb3f167&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprobabilistic-view-of-principal-component-analysis-9c1bbb3f167&source=-----9c1bbb3f167---------------------bookmark_footer-----------)![](../Images/ea43c4d373d4f057fb224899a1bae3d3.png)

寻找隐藏变量（图片来源：[作者](https://flickr.com/photos/suvob/52911155451/))

在数据科学和机器学习中，主要使用的降维技术之一是主成分分析（PCA）。之前，我们已经讨论了在一个[管道](/a-simple-example-of-pipeline-in-machine-learning-with-scikit-learn-e726ffbb6976)中应用PCA的几个例子，其中包括[支持向量机](/visualizing-support-vector-machine-decision-boundary-69e7591dacea)，在这里我们将从概率视角来看待PCA，以提供对数据结构的更稳健和全面的理解。*概率PCA（PPCA）的最大优势之一是它可以处理数据集中的缺失值，而传统PCA则无法做到这一点。* 由于我们将讨论潜在变量模型和期望最大化算法，你还可以查看[这篇详细文章](/latent-variables-expectation-maximization-algorithm-fb15c4e0f32c)。

你可以从这篇文章中学到什么？

1.  主成分分析（PCA）简短介绍。

1.  PPCA的数学构建块。

1.  期望最大化（EM）算法还是变分推断？用于参数估计时该选择哪个？

1.  使用TensorFlow Probability实现PPCA以处理一个玩具数据集。

让我们深入了解吧！

## 1\. 奇异值分解（SVD）和主成分分析（PCA）：
