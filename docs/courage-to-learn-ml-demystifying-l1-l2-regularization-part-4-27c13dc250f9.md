# 勇敢学习机器学习：揭示 L1 和 L2 正则化（第 4 部分）

> 原文：[https://towardsdatascience.com/courage-to-learn-ml-demystifying-l1-l2-regularization-part-4-27c13dc250f9?source=collection_archive---------6-----------------------#2023-12-11](https://towardsdatascience.com/courage-to-learn-ml-demystifying-l1-l2-regularization-part-4-27c13dc250f9?source=collection_archive---------6-----------------------#2023-12-11)

## 探索将 L1 和 L2 正则化作为贝叶斯先验

[](https://amyma101.medium.com/?source=post_page-----27c13dc250f9--------------------------------)[![Amy Ma](../Images/2edf55456a1f92724535a1441fa2bef5.png)](https://amyma101.medium.com/?source=post_page-----27c13dc250f9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----27c13dc250f9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----27c13dc250f9--------------------------------) [Amy Ma](https://amyma101.medium.com/?source=post_page-----27c13dc250f9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6d8df787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-demystifying-l1-l2-regularization-part-4-27c13dc250f9&user=Amy+Ma&userId=d6d8df787b&source=post_page-d6d8df787b----27c13dc250f9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----27c13dc250f9--------------------------------) · 8 分钟阅读·2023年12月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F27c13dc250f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-demystifying-l1-l2-regularization-part-4-27c13dc250f9&user=Amy+Ma&userId=d6d8df787b&source=-----27c13dc250f9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F27c13dc250f9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-demystifying-l1-l2-regularization-part-4-27c13dc250f9&source=-----27c13dc250f9---------------------bookmark_footer-----------)![](../Images/22bdc3089e02a827c9315e4de1e8cd0f.png)

图片由 [Dominik Jirovský](https://unsplash.com/@dominik_jirovsky?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎回到‘[勇敢学习机器学习](/towardsdatascience.com/tagged/courage-to-learn-ml)：揭示 L1 和 L2 正则化’，这是第四篇文章。上次，我们的导师-学习者配对通过 [拉格朗日乘子法的视角探讨了 L1 和 L2 正则化的属性](https://medium.com/p/ee27cd4b557a)。

在关于L1和L2正则化的总结部分，这对将从全新的角度探讨这些话题——[贝叶斯先验](https://medium.com/p/65218b2c2b99)。我们还将总结L1和L2正则化在不同算法中的应用。

在本文中，我们将探讨几个有趣的问题。如果这些话题引起了你的兴趣，你来对地方了！

+   MAP先验如何与L1和L2正则化相关

+   直观地分解使用拉普拉斯和正态分布作为先验

+   理解L1正则化引发的稀疏性及其与拉普拉斯先验的关系

+   兼容L1和L2正则化的算法

+   为什么在神经网络训练中，L2正则化通常被称为“权重衰减”

+   神经网络中不常使用L1范数的原因

# **所以，我们讨论了如何**…
