# 如何在数据可视化中使用颜色

> 原文：[https://towardsdatascience.com/how-to-use-color-in-data-visualizations-37b9752b182d?source=collection_archive---------5-----------------------#2023-10-07](https://towardsdatascience.com/how-to-use-color-in-data-visualizations-37b9752b182d?source=collection_archive---------5-----------------------#2023-10-07)

## **利用颜色提升数据理解**

[](https://medium.com/@michalszudejko?source=post_page-----37b9752b182d--------------------------------)[![Michal Szudejko](../Images/d4c303d02a79ad29df193ed3b25910d9.png)](https://medium.com/@michalszudejko?source=post_page-----37b9752b182d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----37b9752b182d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----37b9752b182d--------------------------------) [Michal Szudejko](https://medium.com/@michalszudejko?source=post_page-----37b9752b182d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd3b37fc311f7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-color-in-data-visualizations-37b9752b182d&user=Michal+Szudejko&userId=d3b37fc311f7&source=post_page-d3b37fc311f7----37b9752b182d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----37b9752b182d--------------------------------) ·15分钟阅读·2023年10月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F37b9752b182d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-color-in-data-visualizations-37b9752b182d&user=Michal+Szudejko&userId=d3b37fc311f7&source=-----37b9752b182d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F37b9752b182d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-use-color-in-data-visualizations-37b9752b182d&source=-----37b9752b182d---------------------bookmark_footer-----------)![](../Images/b292bcc6f1f8c9b6928475f0b172e33d.png)

**长尾猕猴。** 来源：Adobe Stock，[标准许可](https://wwwimages2.adobe.com/content/dam/cc/en/legal/servicetou/Stock-Additional-Terms_en_US_20230915.pdf)。

**上图突出了弗朗索瓦的长尾猕猴，这是一种非常罕见的猴类物种。** 值得注意的是，它们的幼崽拥有鲜艳的橙色毛发，随着成长变为黑色。流行的理论认为，这种橙色使得父母可以在树顶的环境中更容易地监控幼猴¹。这对于迅速识别和应对威胁，例如即将到来的捕食者攻击，至关重要。如果一只成年猕猴能够说话，它可能会喊道：

> 抓住那些橙色的！

**在这里应该向大自然致敬。多亏了色彩的有效使用，最小和最脆弱的生命的存在可能得以保存。**

# **为什么色彩在数据可视化中如此重要？**

我必须承认，我曾犹豫了很长时间才发布这篇文章。原因在于这个话题已经被广泛讨论。然而，我仍然看到一个细分领域，特别是在展示色彩使用的实际细节方面。

我在最近的一篇[文章](https://medium.com/towards-data-science/how-to-talk-about-data-and-analysis-to-non-data-people-2457dc600219)中已经介绍了色彩概念的功能性使用。现在，我想在这个领域分享一些更多的技巧。希望你会觉得这些技巧既实用又公正。让我们从一些无效色彩的例子开始吧……
