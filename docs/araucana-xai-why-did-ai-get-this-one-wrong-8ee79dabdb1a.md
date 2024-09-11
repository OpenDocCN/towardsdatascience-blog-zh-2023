# Araucana XAI：利用决策树进行医疗保健的局部解释

> 原文：[`towardsdatascience.com/araucana-xai-why-did-ai-get-this-one-wrong-8ee79dabdb1a?source=collection_archive---------14-----------------------#2023-07-14`](https://towardsdatascience.com/araucana-xai-why-did-ai-get-this-one-wrong-8ee79dabdb1a?source=collection_archive---------14-----------------------#2023-07-14)

## 介绍了一种基于 CART 的新型模型无关的后期 XAI 方法，以提供局部解释，从而提高 AI 辅助决策的透明度，特别是在医疗保健领域

[](https://medium.com/@detsutut?source=post_page-----8ee79dabdb1a--------------------------------)![Tommaso Buonocore](https://medium.com/@detsutut?source=post_page-----8ee79dabdb1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ee79dabdb1a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ee79dabdb1a--------------------------------) [Tommaso Buonocore](https://medium.com/@detsutut?source=post_page-----8ee79dabdb1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5c63ef55f821&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faraucana-xai-why-did-ai-get-this-one-wrong-8ee79dabdb1a&user=Tommaso+Buonocore&userId=5c63ef55f821&source=post_page-5c63ef55f821----8ee79dabdb1a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ee79dabdb1a--------------------------------) · 7 分钟阅读 · 2023 年 7 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ee79dabdb1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faraucana-xai-why-did-ai-get-this-one-wrong-8ee79dabdb1a&user=Tommaso+Buonocore&userId=5c63ef55f821&source=-----8ee79dabdb1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ee79dabdb1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Faraucana-xai-why-did-ai-get-this-one-wrong-8ee79dabdb1a&source=-----8ee79dabdb1a---------------------bookmark_footer-----------)![](img/22aec8c9f8f98b143838f8990ff864f5.png)

“Araucana” 这个词来源于智利的猴面包树，但它也是一种美丽的家禽品种的名称。© [MelaniMarfeld](https://pixabay.com/photos/araucana-hen-chicken-poultry-4097906/) 取自 Pixabay

# 为什么 AI 会出错？

在人工智能领域，关于复杂 AI 系统缺乏透明性和可理解性的问题引起了越来越多的关注。最近的研究致力于通过开发解释模型来解决这一问题，这些模型揭示了像提升（boosting）、袋装（bagging）和深度学习技术这样的模糊系统的内部工作原理。

## 局部和全局可解释性

解释模型可以以两种不同的方式揭示 AI 系统的行为：

+   **全局可解释性**。全局解释器提供了对 AI 分类器整体行为的*全面理解*。它们旨在揭示在各种输入和场景中始终一致的总体模式、趋势、偏差和其他特征。

+   **局部可解释性**。另一方面，局部解释器侧重于提供*对决策过程的见解*。
