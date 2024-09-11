# 使用深度学习构建强大的推荐系统

> 原文：[https://towardsdatascience.com/building-powerful-recommender-systems-with-deep-learning-d8a919c52119?source=collection_archive---------4-----------------------#2023-07-03](https://towardsdatascience.com/building-powerful-recommender-systems-with-deep-learning-d8a919c52119?source=collection_archive---------4-----------------------#2023-07-03)

![](../Images/8f6471ecbf2d007861bcf4ab5b5aa65a.png)

作者插图

## 使用 PyTorch 库 TorchRec 的逐步实现

[](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)[![Lina Faik](../Images/24a3aa67a2d9dc3e074ceead04ab4cc8.png)](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------) [Lina Faik](https://linafaik.medium.com/?source=post_page-----d8a919c52119--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb6c0e8e98c84&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-powerful-recommender-systems-with-deep-learning-d8a919c52119&user=Lina+Faik&userId=b6c0e8e98c84&source=post_page-b6c0e8e98c84----d8a919c52119---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8a919c52119--------------------------------) ·7 分钟阅读·2023年7月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8a919c52119&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-powerful-recommender-systems-with-deep-learning-d8a919c52119&user=Lina+Faik&userId=b6c0e8e98c84&source=-----d8a919c52119---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8a919c52119&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-powerful-recommender-systems-with-deep-learning-d8a919c52119&source=-----d8a919c52119---------------------bookmark_footer-----------)

在正确的时间向客户推荐合适的产品是各个行业普遍面临的挑战。例如，银行家不断寻求向现有或潜在客户推荐高度相关的服务。零售商努力推荐符合客户口味的吸引人产品。同样，社交网络也致力于构建引人入胜的内容流以促进用户采纳。

尽管这是一个被广泛研究的用例，但由于问题的独特性质，取得令人满意的性能结果仍然困难重重。主要原因包括存在大量的类别数据，这往往导致稀缺问题，以及用例的计算方面，这造成了可扩展性问题。直到最近，推荐模型才开始利用神经网络。

在这种背景下，Meta 开发并公开了一个深度学习推荐模型（DRLM）。该模型特别值得注意，因为它将协同过滤和预测分析的原则结合起来，并且适用于大规模生产。

## **目标**
