# 探索聚类领域

> 原文：[https://towardsdatascience.com/navigating-the-clustering-landscape-b7930ac44147?source=collection_archive---------16-----------------------#2023-05-29](https://towardsdatascience.com/navigating-the-clustering-landscape-b7930ac44147?source=collection_archive---------16-----------------------#2023-05-29)

## 聚类 | 机器学习 | Python

## 主要聚类算法的比较及实际 Python 示例

[](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)[![David Farrugia](../Images/082ed61e24c7c26a4ae1c77343a87824.png)](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----b7930ac44147--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3916826092a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnavigating-the-clustering-landscape-b7930ac44147&user=David+Farrugia&userId=3916826092a6&source=post_page-3916826092a6----b7930ac44147---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b7930ac44147--------------------------------) ·7 min read·2023年5月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb7930ac44147&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnavigating-the-clustering-landscape-b7930ac44147&user=David+Farrugia&userId=3916826092a6&source=-----b7930ac44147---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb7930ac44147&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnavigating-the-clustering-landscape-b7930ac44147&source=-----b7930ac44147---------------------bookmark_footer-----------)![](../Images/95b6eab856ff3a4e575767f07f2d4ebe.png)

照片由 [Aleks Dorohovich](https://unsplash.com/ja/@doctype?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

机器学习领域不断发展，并在近年来引起了高度关注。特别是当前的 AI 热潮，数据科学和机器学习的神秘世界无疑从广泛的公众曝光中受益匪浅。

在这篇文章中，我们将把焦点放在机器学习的一个迷人子领域——聚类上。

让我们尝试描绘一个画面，好吗？

假设你站在一个非常繁忙的城市中心，周围是成千上万的人。每个人在那一刻都是100%独特的。他们是独立的个体——然而，你们都有一些共同的特点或特质将你们联系在一起。也许是你的时尚感、职业、政治或宗教信仰，甚至是你喜欢的食物。

这就是聚类的本质——在数据点的海洋中寻找隐藏的模式和分组。在机器学习的世界里，聚类技术是数据宇宙的制图师，绘制出信息的星座，否则这些信息可能看起来...
