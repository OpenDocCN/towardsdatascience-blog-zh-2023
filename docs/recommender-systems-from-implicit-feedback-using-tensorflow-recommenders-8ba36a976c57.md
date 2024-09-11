# 使用 TensorFlow 推荐器从隐式反馈中构建推荐系统

> 原文：[https://towardsdatascience.com/recommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57?source=collection_archive---------4-----------------------#2023-11-05](https://towardsdatascience.com/recommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57?source=collection_archive---------4-----------------------#2023-11-05)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 当客户没有明确告诉你他们想要什么时

[](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----8ba36a976c57--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----8ba36a976c57---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ba36a976c57--------------------------------) ·11分钟阅读·2023年11月5日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ba36a976c57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----8ba36a976c57---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ba36a976c57&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frecommender-systems-from-implicit-feedback-using-tensorflow-recommenders-8ba36a976c57&source=-----8ba36a976c57---------------------bookmark_footer-----------)![](../Images/085198f24d7254f94814a1ebdec4619b.png)

图片来源 [Noom Peerapong](https://unsplash.com/@imnoom?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

进行推荐其实并没有那么难。你只需要检查客户如何评价你的产品，例如使用1到5颗星，然后在其基础上训练一个**回归**模型。对吗？

![](../Images/ae0ec03fd0b1941c5147d59abe3a80c0.png)

你希望拥有的典型数据集。图像由作者提供。

好的，如果我们没有任何数值的用户或电影特征，我们可能需要处理嵌入，但在我之前的文章中已经展示了如何做到这一点：

[## 嵌入式推荐系统简介](https://towardsdatascience.com/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----8ba36a976c57--------------------------------)

### 学习在 TensorFlow 中构建简单的矩阵分解推荐系统

towardsdatascience.com](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----8ba36a976c57--------------------------------)

本文还需要使用嵌入，因此我建议在继续之前阅读上面的文章。

# 隐式反馈

但是，有时候我们并不幸运地拥有**明确的**用户反馈，如星级、点赞或点踩，或类似的反馈。这种情况经常发生在…
