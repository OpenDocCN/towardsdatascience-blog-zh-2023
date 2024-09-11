# 如何使用 Polars 构建基于流行度的推荐系统

> 原文：[https://towardsdatascience.com/how-to-build-popularity-based-recommenders-with-polars-cc7920ad3f68?source=collection_archive---------7-----------------------#2023-04-28](https://towardsdatascience.com/how-to-build-popularity-based-recommenders-with-polars-cc7920ad3f68?source=collection_archive---------7-----------------------#2023-04-28)

## [推荐系统](https://medium.com/tag/recommendation-system)

## 基本推荐系统易于理解和实现，并且训练速度快

[](https://dr-robert-kuebler.medium.com/?source=post_page-----cc7920ad3f68--------------------------------)[![Dr. Robert Kübler](../Images/3b8d8b88f76c0c43d9c305e3885e7ab9.png)](https://dr-robert-kuebler.medium.com/?source=post_page-----cc7920ad3f68--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cc7920ad3f68--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----cc7920ad3f68--------------------------------) [Dr. Robert Kübler](https://dr-robert-kuebler.medium.com/?source=post_page-----cc7920ad3f68--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6d6b5fb431bf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-popularity-based-recommenders-with-polars-cc7920ad3f68&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=post_page-6d6b5fb431bf----cc7920ad3f68---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cc7920ad3f68--------------------------------) ·6 min read·2023年4月28日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fcc7920ad3f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-popularity-based-recommenders-with-polars-cc7920ad3f68&user=Dr.+Robert+K%C3%BCbler&userId=6d6b5fb431bf&source=-----cc7920ad3f68---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fcc7920ad3f68&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-build-popularity-based-recommenders-with-polars-cc7920ad3f68&source=-----cc7920ad3f68---------------------bookmark_footer-----------)![](../Images/86cf3808c7ceac75c67a41bd0c28464e.png)

由我创建于 [dreamstudio.ai](https://beta.dreamstudio.ai/).

推荐系统是设计用来根据用户的过去行为、偏好和互动来提供推荐的算法。它们已经成为包括电子商务、娱乐和广告在内的各个行业不可或缺的组成部分，提升了用户体验，增加了客户留存率，并推动了销售。

虽然存在各种先进的推荐系统，但今天我想向您展示其中一个最直接的——但通常很难超越的推荐系统：**基于流行度的推荐系统**。它是一个很好的基准推荐系统，您应该在尝试更高级的模型（如矩阵分解）之外始终尝试它。

[](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----cc7920ad3f68--------------------------------) [## 嵌入式推荐系统简介

### 学会在TensorFlow中构建简单的矩阵分解推荐系统

towardsdatascience.com](/introduction-to-embedding-based-recommender-systems-956faceb1919?source=post_page-----cc7920ad3f68--------------------------------)

我们将在本文中使用[polars](https://pola.rs/)创建*两种*不同类型的基于流行度的推荐系统。如果您以前没有使用过快速的pandas替代品polars，不用担心；本文是一个很好的...
