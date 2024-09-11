# 方便的贝叶斯营销组合建模与 PyMC 营销

> 原文：[https://towardsdatascience.com/convenient-bayesian-marketing-mix-modeling-with-pymc-marketing-8b02a9a9c4aa?source=collection_archive---------3-----------------------#2023-04-13](https://towardsdatascience.com/convenient-bayesian-marketing-mix-modeling-with-pymc-marketing-8b02a9a9c4aa?source=collection_archive---------3-----------------------#2023-04-13)

## [营销分析](https://medium.com/tag/marketing-analytics)

## PyMC 团队推出了一个全新而闪亮的库，值得尝试

[](https://dr-robert-kuebler.medium.com/?source=post_page-----8b02a9a9c4aa--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----8b02a9a9c4aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8b02a9a9c4aa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8b02a9a9c4aa--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----8b02a9a9c4aa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvenient-bayesian-marketing-mix-modeling-with-pymc-marketing-8b02a9a9c4aa&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----8b02a9a9c4aa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8b02a9a9c4aa--------------------------------) · 6 分钟阅读 · 2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8b02a9a9c4aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvenient-bayesian-marketing-mix-modeling-with-pymc-marketing-8b02a9a9c4aa&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----8b02a9a9c4aa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8b02a9a9c4aa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fconvenient-bayesian-marketing-mix-modeling-with-pymc-marketing-8b02a9a9c4aa&source=-----8b02a9a9c4aa---------------------bookmark_footer-----------)![](../Images/f28f4c3f55d755db249a94b0f4d38e6a.png)

照片由 [Nathan Fertig](https://unsplash.com/@nathanfertig?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

你可以通过有多少大公司在发布相关软件包来判断一个主题的重要性。在营销组合建模领域，

+   Google 发布了 [LMMM](https://github.com/google/lightweight_mmm)

+   Meta 发布了 [Robyn](https://github.com/facebookexperimental/Robyn)

+   PyMC Labs 发布了[PyMC Marketing](https://github.com/pymc-labs/pymc-marketing)

+   （而且我发布了[mamimo](https://github.com/Garve/mamimo) 😇）

比市场营销混合建模更好的的是**贝叶斯市场营销混合建模**，这是 Google 和 PyMC Labs 的库所提供的。虽然 LMMM 也确实很有趣，但今天我们将专注于 PyMC Marketing。

在这篇文章中，你将了解到如今构建最先进的贝叶斯市场营销混合模型是多么简单！

如果你需要复习，请查看我以前的文章，它介绍了贝叶斯市场营销混合建模的所有内容。

[](/bayesian-marketing-mix-modeling-in-python-via-pymc3-7b2071f6001a?source=post_page-----8b02a9a9c4aa--------------------------------) [## 使用 PyMC3 在 Python 中进行贝叶斯市场营销混合建模

### 一次性估计饱和度、滞后效应和其他参数，包括它们的不确定性

towardsdatascience.com](/bayesian-marketing-mix-modeling-in-python-via-pymc3-7b2071f6001a?source=post_page-----8b02a9a9c4aa--------------------------------)

# 手动过程
