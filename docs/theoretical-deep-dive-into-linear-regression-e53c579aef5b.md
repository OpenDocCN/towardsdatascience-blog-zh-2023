# 线性回归的理论深度探讨

> 原文：[https://towardsdatascience.com/theoretical-deep-dive-into-linear-regression-e53c579aef5b?source=collection_archive---------1-----------------------#2023-06-23](https://towardsdatascience.com/theoretical-deep-dive-into-linear-regression-e53c579aef5b?source=collection_archive---------1-----------------------#2023-06-23)

## [可解释的人工智能](https://medium.com/tag/explainable-ai)

## 了解线性回归的本质以及如何以各种自然的方式扩展它

[](https://dr-robert-kuebler.medium.com/?source=post_page-----e53c579aef5b--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----e53c579aef5b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e53c579aef5b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e53c579aef5b--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----e53c579aef5b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftheoretical-deep-dive-into-linear-regression-e53c579aef5b&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----e53c579aef5b---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----e53c579aef5b--------------------------------) ·10分钟阅读·2023年6月23日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe53c579aef5b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftheoretical-deep-dive-into-linear-regression-e53c579aef5b&source=-----e53c579aef5b---------------------bookmark_footer-----------)![](../Images/8b8ab36b31e37bf56824bc5f7b55d548.png)

图片由[Erik van Dijk](https://unsplash.com/@erikvandijk?utm_source=medium&utm_medium=referral)提供，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数有抱负的数据科学博客作者都会这样做：写一篇关于线性回归的介绍文章——这是一种自然的选择，因为这是我们进入该领域时学习的第一个模型之一。虽然这些文章对初学者很有帮助，但大多数文章并没有深入到足以满足高级数据科学家的程度。

所以，让我带你了解一些鲜为人知但令人耳目一新的线性回归细节，这将使你成为更优秀的数据科学家（并在面试中为你加分）。

*这篇文章涉及很多数学内容，因此有一定的概率和微积分基础会更有帮助。*

# 数据生成过程

我在建模时非常喜欢考虑数据生成过程。接触过贝叶斯建模的人会明白我的意思，但对于其他人：想象你有一个数据集（*X*，*y*），由样本（*x*，*y*）组成。给定* x*，如何得到一个目标* y*？

> *假设我们有* n *个数据点，并且每个* x *有* k *个组件/特征。*
