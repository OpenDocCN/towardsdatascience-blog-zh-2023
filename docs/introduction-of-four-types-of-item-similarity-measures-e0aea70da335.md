# 四种项目相似性度量的介绍

> 原文：[https://towardsdatascience.com/introduction-of-four-types-of-item-similarity-measures-e0aea70da335?source=collection_archive---------11-----------------------#2023-02-17](https://towardsdatascience.com/introduction-of-four-types-of-item-similarity-measures-e0aea70da335?source=collection_archive---------11-----------------------#2023-02-17)

## 数据挖掘

## 讲解如何在项目嵌入可用时选择相似性度量

[](https://medium.com/@jhwang1992m?source=post_page-----e0aea70da335--------------------------------)[![Jiahui Wang](../Images/91350774d661092f429d1b0591af95f4.png)](https://medium.com/@jhwang1992m?source=post_page-----e0aea70da335--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0aea70da335--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e0aea70da335--------------------------------) [Jiahui Wang](https://medium.com/@jhwang1992m?source=post_page-----e0aea70da335--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4037e6e33535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-of-four-types-of-item-similarity-measures-e0aea70da335&user=Jiahui+Wang&userId=4037e6e33535&source=post_page-4037e6e33535----e0aea70da335---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0aea70da335--------------------------------) ·5分钟阅读·2023年2月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0aea70da335&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-of-four-types-of-item-similarity-measures-e0aea70da335&user=Jiahui+Wang&userId=4037e6e33535&source=-----e0aea70da335---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0aea70da335&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-of-four-types-of-item-similarity-measures-e0aea70da335&source=-----e0aea70da335---------------------bookmark_footer-----------)![](../Images/b75c9c4e4fd412a9e5b9ad5ec3a51357.png)

图片由 [James Yarema](https://unsplash.com/@jamesyarema?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

推荐算法在我们日常生活中无处不在，从我们 Youtube 首页上出现的视频顺序，到 Walmart 商品架上的排列。这些算法学习用户的潜在偏好，并向用户推荐最相关的项目。为了实现这一点，我们需要将用户和项目描述为数值向量，以便算法可以测量用户相似度和项目相似度，从而做出进一步的推荐。

学习用户和项目的数值向量表示有两种常见方法：[基于内容的过滤和协同过滤](https://developers.google.com/machine-learning/recommendation/overview/candidate-generation)。在基于内容的过滤中，项目的向量表示是通过根据项目的专家知识手动构建特征来学习的。以移动应用为例，Google Play 上的每个应用都有一个应用名称、一个类别和一些开发者提供的文本描述。然后可以应用专家知识，从这些信息中提取特征，以构建应用的向量表示。基于内容的过滤的优点在于它不依赖于现有的用户-项目数据。相比之下…
