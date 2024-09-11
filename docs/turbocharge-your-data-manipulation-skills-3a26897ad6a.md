# 让你的数据处理技能获得极大提升

> 原文：[https://towardsdatascience.com/turbocharge-your-data-manipulation-skills-3a26897ad6a?source=collection_archive---------5-----------------------#2023-02-21](https://towardsdatascience.com/turbocharge-your-data-manipulation-skills-3a26897ad6a?source=collection_archive---------5-----------------------#2023-02-21)

## 解锁 pandas 的 groupby、apply 和 transform 的强大功能

[](https://bradley-stephen-shaw.medium.com/?source=post_page-----3a26897ad6a--------------------------------)[![Bradley Stephen Shaw](../Images/b3ef5e6e292083ff0f8523ec5ffe89f0.png)](https://bradley-stephen-shaw.medium.com/?source=post_page-----3a26897ad6a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3a26897ad6a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3a26897ad6a--------------------------------) [Bradley Stephen Shaw](https://bradley-stephen-shaw.medium.com/?source=post_page-----3a26897ad6a--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc5cd0a58b5ae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturbocharge-your-data-manipulation-skills-3a26897ad6a&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=post_page-c5cd0a58b5ae----3a26897ad6a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3a26897ad6a--------------------------------) ·11 min read·2023年2月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3a26897ad6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturbocharge-your-data-manipulation-skills-3a26897ad6a&user=Bradley+Stephen+Shaw&userId=c5cd0a58b5ae&source=-----3a26897ad6a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3a26897ad6a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fturbocharge-your-data-manipulation-skills-3a26897ad6a&source=-----3a26897ad6a---------------------bookmark_footer-----------)![](../Images/4f99de58d30f656f07cc060f24d46e5e.png)

由 [Kier in Sight](https://unsplash.com/@kierinsight?utm_source=medium&utm_medium=referral) 提供的照片，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在一个竞争激烈且数据丰富的世界中，理解分段行为是提供量身定制的见解和产品的关键。

无论是通过描述性统计理解分段趋势，还是通过包括分段特征在机器学习模型中等更细致的方法，都需要进行一些数据处理。

幸运的是，pandas 提供了非常多功能的功能，使我们能够轻松处理用于以各种方式分割数据的大部分繁重操作。通过一些示例，我们将演示：

1.  `groupby` 操作——它是什么，它如何工作，以及它返回什么。

1.  如何与 `groupby` 一起使用 `apply` 以进行更复杂和独特的转换。

1.  使用 `groupby` 和 `transform` 将 `groupby` 和 `apply` 的魔法映射回原始数据形状。

1.  我在过程中学到的一些技巧和窍门。

让我们开始吧——首先，获取一些数据来进行操作。

# 数据

这次我们将使用从一组消费者信用卡¹中收集的信息。
