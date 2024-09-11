# 形态学操作与模拟（CV-05）

> 原文：[https://towardsdatascience.com/morphological-operations-a-way-to-remove-image-distortion-513d162e7d05?source=collection_archive---------14-----------------------#2023-05-10](https://towardsdatascience.com/morphological-operations-a-way-to-remove-image-distortion-513d162e7d05?source=collection_archive---------14-----------------------#2023-05-10)

## 图像处理中的形态学操作的最简单解释

[](https://zubairhossain.medium.com/?source=post_page-----513d162e7d05--------------------------------)[![Md. Zubair](../Images/1b983a23226ce7561796fa5b28c00d65.png)](https://zubairhossain.medium.com/?source=post_page-----513d162e7d05--------------------------------)[](https://towardsdatascience.com/?source=post_page-----513d162e7d05--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----513d162e7d05--------------------------------) [Md. Zubair](https://zubairhossain.medium.com/?source=post_page-----513d162e7d05--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fdaeaeeea52&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmorphological-operations-a-way-to-remove-image-distortion-513d162e7d05&user=Md.+Zubair&userId=2fdaeaeeea52&source=post_page-2fdaeaeeea52----513d162e7d05---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----513d162e7d05--------------------------------) ·8分钟阅读·2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F513d162e7d05&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmorphological-operations-a-way-to-remove-image-distortion-513d162e7d05&user=Md.+Zubair&userId=2fdaeaeeea52&source=-----513d162e7d05---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F513d162e7d05&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmorphological-operations-a-way-to-remove-image-distortion-513d162e7d05&source=-----513d162e7d05---------------------bookmark_footer-----------)![](../Images/25ac9ee05dcdbe22e425ab418321eb3e.png)

图片来源：[Katharina Matt](https://unsplash.com/ko/@katharinamatt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 动力

每天，我们处理大量的图像。这些图像具有不同的强度和分辨率。有时候，我们无法从图像中提取出合适的信息。各种图像处理技术在许多方面都能帮助我们。形态学操作是其中一种重要技术，它可以减少图像的失真并改变形状。请查看下面的图像。

![](../Images/4c9d05405e8a0f4dd3711870c326274e.png)

左边是应用形态学操作前的图像，右边是操作后的图像（图片来源：作者）

另一个例子 —

![](../Images/0d51522cf10f5cc4fca5cc584a6b73cc.png)

应用形态学操作前后的对比（图片来源：作者）

结果很有趣。但形态学操作的应用场景不仅限于这两种任务。*在这篇文章中，我将讨论你需要了解的所有关于形态学操作的内容。*

`*[注意：主要的形态学分析适用于二值图像。如果你不知道如何创建二值图像，请阅读我之前关于* [*图像阈值处理*](https://medium.com/towards-data-science/thresholding-a-way-to-make-images-more-visible-b3e314b5215c)*的文章。]*`
