# 避免那三种昂贵的错误，以拯救你的 A/B 测试

> 原文：[`towardsdatascience.com/save-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22?source=collection_archive---------17-----------------------#2023-07-07`](https://towardsdatascience.com/save-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22?source=collection_archive---------17-----------------------#2023-07-07)

## 细节决定成败

[](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)![Quentin Gallea, PhD](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------) [Quentin Gallea, PhD](https://medium.com/@quentin.gallea?source=post_page-----6ff2e4effe22--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa52dcb9793ad&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsave-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22&user=Quentin+Gallea%2C+PhD&userId=a52dcb9793ad&source=post_page-a52dcb9793ad----6ff2e4effe22---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ff2e4effe22--------------------------------) ·5 min read·2023 年 7 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ff2e4effe22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsave-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22&user=Quentin+Gallea%2C+PhD&userId=a52dcb9793ad&source=-----6ff2e4effe22---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ff2e4effe22&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsave-your-a-b-testing-by-avoiding-those-3-costly-mistakes-6ff2e4effe22&source=-----6ff2e4effe22---------------------bookmark_footer-----------)![](img/328f6861b0e50d534a46abdf88915ce6.png)

图片由 [Christian Stahl](https://unsplash.com/@woodpecker65?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/fr/photos/8S96OpxSlvg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着随机对照试验的应用从学术界（特别是医学研究）扩展到商业领域，它们现在成为了企业做出数据驱动决策的流行方法。特别是，在线 A/B 测试易于实施，并且在优化数字流程方面潜力巨大。通过比较两个或多个变体，组织可以评估不同选项的有效性，并确定最有利的结果。然而，认识并解决某些限制至关重要，以确保偏差不会影响结果的可靠性和有效性。在本文中，我们探讨了在进行在线 A/B 测试之前需要考虑的三个关键限制，以避免成本高昂的偏差。在深入列出我个人认为的三大问题之前，让我简要定义 A/B 测试及其重要概念。

## 什么是 A/B 测试？

A/B 测试涉及向不同的研究对象（例如客户）展示不同的版本/变体 A 和 B。在线 A/B 测试可以探索网页、电子邮件营销、用户界面或任何其他数字资产的变体给用户子集。这些变体通常在一个或多个特定元素上有所不同，例如设计、布局、颜色方案、行动号召等。
