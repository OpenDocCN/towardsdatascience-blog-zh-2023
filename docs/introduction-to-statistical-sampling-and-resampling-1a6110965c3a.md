# 统计抽样与重抽样简介

> 原文：[`towardsdatascience.com/introduction-to-statistical-sampling-and-resampling-1a6110965c3a?source=collection_archive---------6-----------------------#2023-05-16`](https://towardsdatascience.com/introduction-to-statistical-sampling-and-resampling-1a6110965c3a?source=collection_archive---------6-----------------------#2023-05-16)

## *统计抽样是统计学的一个基础模块，它使我们能够高效地获取感兴趣的群体信息，而无需直接研究整个群体*

[](https://medium.com/@theDrewDag?source=post_page-----1a6110965c3a--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----1a6110965c3a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a6110965c3a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a6110965c3a--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----1a6110965c3a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4e8f67b0b09b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-statistical-sampling-and-resampling-1a6110965c3a&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=post_page-4e8f67b0b09b----1a6110965c3a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a6110965c3a--------------------------------) ·10 分钟阅读·2023 年 5 月 16 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a6110965c3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-statistical-sampling-and-resampling-1a6110965c3a&user=Andrea+D%27Agostino&userId=4e8f67b0b09b&source=-----1a6110965c3a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a6110965c3a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-statistical-sampling-and-resampling-1a6110965c3a&source=-----1a6110965c3a---------------------bookmark_footer-----------)![](img/cb0b46ca1f49582351ced40a83cb95f9.png)

照片由 [Testalize.me](https://unsplash.com/@testalizeme?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

任何研究者最隐秘的愿望之一就是能够拥有他们打算研究的整个群体的数据。

研究整个群体使研究者能够对研究现象获得全面的理解，因为这允许收集关于构成该群体的所有个体的信息。

在大多数情况下，这在实践和理论上都是不切实际的。

例如，让我们考虑一个由意大利罗马市的居民组成的人群。如果我们的研究需要包括这些个体的回应，由于现实世界的限制，**定位和联系他们所有人可能是不可能的**。

我们需要定位并请求他们的回应——这是对罗马所有个体的要求。

这可能会证明过于昂贵且耗时，无论是对于单独工作的研究人员还是团队。

因此，**通常需要使用样本作为近似值**…
